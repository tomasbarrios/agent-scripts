# Dev servers multi-proyecto (Tailscale)

**Regla:** en esta máquina corren varios proyectos a la vez. Antes de levantar un
dev server, consultar el índice y dar la **URL Tailscale** del proyecto. No
spawnear un segundo Vite/Remix “por si acaso”.

Fuente de datos vivos (puerto, URL, cwd, comando):
[`~/own/multi-project-router/INDEX.md`](/home/tomas/own/multi-project-router/INDEX.md)
§ Dev servers (Tailscale) y el bloque `dev:` de cada proyecto.

## Por qué

- Varios repos pueden tener `pnpm run dev` / `npm run dev` ya vivos.
- Vite, si el puerto preferido está ocupado, abre el siguiente (`5174`, …) y
  deja un duplicado inútil que **no** está detrás de Tailscale Serve.
- Tomás prueba desde el teléfono u otra máquina del tailnet; la URL útil es
  `https://tomas-macmini.tail23270a.ts.net:<puerto>`, no `localhost`.

## Procedimiento

Cuando el usuario pida ver, probar, abrir o revisar la app en el navegador:

1. Identificar el proyecto (contexto del thread o scoring del router).
2. Leer el bloque `dev:` de ese proyecto en `INDEX.md` (y la tabla Tailscale).
3. Comprobar si el `port` local ya escucha **y** el cwd del proceso coincide
   con `dev.cwd` (p. ej. `ss`/`lsof` + `/proc/<pid>/cwd`).
4. Si está vivo → responder solo con `dev.url` (markdown link). Listo.
5. Si no hay listener → levantar **una** vez con `dev.command` en `dev.cwd`,
   esperar a que el puerto documentado esté up, y dar `dev.url`.
6. Si el puerto preferido está ocupado por **otro** cwd → preguntar; no abrir
   un segundo puerto ni inventar otra URL Tailscale.

## HMR de Vite detrás de otro server

Si un proyecto corre Vite detrás de otro server (p. ej. Rails + `vite_rails`),
el `dev.url` normal no alcanza para hot reload en remoto: el cliente de HMR
abre un websocket directo al puerto del dev server de Vite, no al del server
principal, y ese puerto casi nunca está en Tailscale Serve.

Estándar: puerto `85XX` reservado por proyecto para ese websocket (ver
`INDEX.md` § Rango 85XX). Si el proyecto define `dev.hmr_url`, darlo junto al
`dev.url` cuando el usuario vaya a usar hot reload desde otra máquina del
tailnet.

## Anti-patrones

- Empezar con `pnpm run dev` sin mirar el router ni los puertos.
- Pegar `http://localhost:…` como URL principal (solo como dato diagnóstico
  si hace falta).
- Asumir que “port in use → next port” es aceptable: ese next port no está en
  `tailscale serve`.
- Exponer con `bb connect` en lugar de la URL Tailscale ya documentada, salvo
  que el usuario lo pida o Tailscale no aplique a ese servicio.

## Mantenimiento

Al agregar un proyecto con UI local, o al cambiar puerto / Serve:

1. Actualizar `tailscale serve` en el Mac mini.
2. Actualizar la tabla y el `dev:` en `multi-project-router/INDEX.md`.
3. Opcional: el mismo bloque en el `ROUTING.md` del proyecto.

La regla de agente no guarda puertos: solo apunta al índice.
