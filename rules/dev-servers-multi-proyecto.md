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

## HMR / live-reload detrás de otro server

Si el cliente de hot reload abre un websocket a un puerto distinto del de la
app (Vite detrás de Rails, Remix LiveReload en `:8002`, etc.), el `dev.url`
normal no alcanza para reload en remoto.

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

Al agregar un proyecto con UI local, o al cambiar puerto / Serve, completar
los 6 pasos (si falta uno, el server queda vivo pero invisible o sin
autoarranque):

- [ ] 1. **`INDEX.md`:** fila en la tabla Tailscale (y en § Rango 85XX si hay
  HMR) + bloque `dev:` del proyecto en `~/own/multi-project-router/INDEX.md`.
- [ ] 2. **`ROUTING.md` espejo:** el mismo bloque `dev:` (cwd, command,
  package_manager, port, url, hmr_url) en el `ROUTING.md` del proyecto.
- [ ] 3. **Serve:** línea `tailscale serve --bg --https=<puerto público> http://127.0.0.1:<puerto local>`
  en `~/.local/bin/tailscale-serve-setup` (validar con `bash -n`).
- [ ] 4. **Portal:** tarjeta en el portal Caddy
  (`~/.local/caddy/portal/index.html`) con la URL Tailscale del proyecto.
- [ ] 5. **Systemd (si es permanente):** unidad de usuario
  `~/.config/systemd/user/<proyecto>.service` que ejecute un wrapper
  `~/.local/bin/<proyecto>-dev` (cwd + comando + puerto fijo), con
  `WantedBy=default.target`; activar con `systemctl --user enable <proyecto>`.
- [ ] 6. **Orden de arranque:** agregar la unidad a `After=` y `Wants=` de
  `~/.config/systemd/user/tailscale-serve.service`.

La regla de agente no guarda puertos: solo apunta al índice.
