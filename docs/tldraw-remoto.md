# tldraw offline con Claude remoto

Hablas con un Claude que corre en el Mac mini y edita ahí tus archivos, pero el
diagrama se dibuja en el tldraw de tu Mac, al frente tuyo.

## El mapa

```
Tu Mac                                   Mac mini
──────                                   ────────
tldraw Desktop                           Claude Code
  ↑ 127.0.0.1:7236                            ↓
  └──── tldraw-tunel ──────────────→ localhost:7236    + tus .md en ~/own/ideas/mentor
```

El canvas escucha **solo en loopback**. El túnel inverso de SSH lo publica como
`localhost` dentro del Mac mini: para el Claude de allá es indistinguible de
tener tldraw local, y el tráfico va cifrado por SSH. No se abre ningún puerto.

> **Nunca expongas el 7236 a la red.** Ese servidor tiene un endpoint `/exec`
> que ejecuta JavaScript arbitrario en tu Mac. Está pensado para loopback;
> publicarlo es regalar ejecución remota de código. Siempre por túnel.

## Uso diario

**1. Abre tldraw** en tu Mac. Sin la app corriendo no hay nada que publicar.

**2. Levanta el túnel:**

```
tldraw-tunel up
tldraw-tunel status     # confirma que el canvas se alcanza desde el mini
```

**3. Habla con el Claude remoto** como lo hagas normalmente. Ya ve el canvas.

Si prefieres que el comando te deje directo en una sesión de trabajo:

```
mentor-remoto           # túnel + tmux + claude en ~/own/ideas/mentor
```

`Ctrl-b d` sale dejando la sesión viva en el mini.

## Cuando algo falla

| Síntoma | Causa | Solución |
|---|---|---|
| "no puedo conectarme al canvas" | el túnel no está arriba | `tldraw-tunel up` |
| las llamadas devuelven **401** | reiniciaste tldraw y el token rotó | `tldraw-tunel up` (lo refresca) |
| **connection refused** | se cayó el túnel o cerraste tldraw | `tldraw-tunel status`, luego `up` |
| "remote port forwarding failed" | el puerto sigue tomado por una conexión anterior | espera unos segundos y reintenta |
| te pide "To authenticate, visit: https://login.tailscale.com/..." | Tailscale SSH exige reautenticación cada cierto tiempo | abre el link y aprueba; el comando sigue solo |
| "Timeout, server not responding" en el log | el mini durmió o se cortó la red | `tldraw-tunel up` |

Regla práctica: **casi todo se arregla con `tldraw-tunel up`.** Es idempotente —
si ya había túnel, lo reemplaza y de paso refresca el token.

## Lo que el túnel no sobrevive

Se cae con: reinicio del Mac, suspensión de la máquina, o cierre de sesión.
Después de cualquiera de esas, `tldraw-tunel up` de nuevo.

Si algún día molesta repetirlo, la solución es un agente de `launchd` que lo
levante al arrancar y se recupere solo. No está hecho.

## Dos cosas que NO funcionan desde el remoto

Son límites reales, no bugs. Están escritos en el skill del mini para que el
Claude de allá no los intente:

- **Screenshots.** `api.getScreenshot()` devuelve una ruta de archivo *en tu
  Mac*; el mini no tiene ese filesystem. Se verifica con `api.getShapes()` — y
  además tú estás mirando el canvas en vivo, que es mejor que cualquier captura.
- **Document scripts** (`/script-workspace`), o sea comportamiento durable: UI
  clickeable, animaciones, lógica "al abrir". También devuelve rutas de tu Mac.
  Eso hay que hacerlo con Claude corriendo local.

Dibujar, mover, conectar y estilar formas —todo el trabajo de diagramar— sí
funciona completo.

## Las piezas

| Dónde | Archivo | Para qué |
|---|---|---|
| Mac | `~/.local/bin/tldraw-tunel` | levanta, revisa y baja el túnel |
| Mac | `~/.local/bin/mentor-remoto` | túnel + tmux + claude, en un comando |
| Mac mini | `~/skills/tldraw-offline/tq` | habla con la API del canvas |
| Mac mini | `~/.claude/skills/tldraw-offline/SKILL.md` | el skill, con los límites del modo remoto |
| Mac mini | `~/.cache/tldraw-tunnel.json` | puerto y token de la sesión actual |

El token vive en archivo y no en variable de entorno a propósito: una sesión
`tmux` ya abierta hereda el entorno de cuando se creó, así que un token en
variable quedaría viejo sin que se note. En archivo, `tq` lo relee en cada
llamada.
