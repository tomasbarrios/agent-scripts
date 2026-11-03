# Fuente única de instrucciones

**Regla:** en los proyectos del router, las instrucciones de agente viven en
markdown agnóstico del provider (`AGENTS.md` o `docs/agents/*.md`). Los
archivos propios de cada herramienta (`CLAUDE.md`, `.cursor/rules/*.mdc`) son
solo adaptadores que apuntan a esa fuente; nunca contienen reglas propias.

## Alcance

Aplica solo a los proyectos listados en
[`~/own/multi-project-router/INDEX.md`](/home/tomas/own/multi-project-router/INDEX.md).
Las demás carpetas quedan fuera mientras no se registren ahí.

## Sub-reglas

1. **Fuente.** La instrucción vive en `AGENTS.md` (el global en
   `~/Projects/agent-scripts/AGENTS.md` y uno por repo) o en un `.md` de
   `docs/agents/` referenciado desde ese `AGENTS.md`.
2. **CLAUDE.md.** El `CLAUDE.md` de cada repo (y de cada subcarpeta con
   `AGENTS.md` propio) contiene solo `@AGENTS.md`, más lo estrictamente
   propio de Claude Code (si lo hubiera).
3. **Cursor (`.mdc`).** Los `.cursor/rules/*.mdc` son solo adaptadores por
   glob: sin `alwaysApply: true` y con una línea `Fuente: <ruta.md>` que
   apunte a un archivo que exista. El contenido de la regla está en esa
   fuente, no en el `.mdc`.
4. **Regla nueva.** Un agente que quiera crear una regla la escribe en
   markdown (`AGENTS.md` o `docs/agents/`), no en un formato de herramienta.
   Si sirve para más de un proyecto, la **propone** en agent-scripts en vez
   de crearla en el repo (ver [`reglas-globales.md`](reglas-globales.md)).
5. **Derivados.** Los archivos derivados (adaptadores `.mdc`, `CLAUDE.md`) no
   se editan a mano para cambiar una regla: se cambia la fuente.
6. **Validación.** Se valida con `~/Projects/agent-scripts/bin/agents-check`
   (solo lectura; `--help` para opciones, `--strict` para exit 1 si hay
   errores).

## Por qué

Cada herramienta lee un archivo distinto (Claude Code `CLAUDE.md`, Cursor
`.cursor/rules/`, Codex/OpenCode `AGENTS.md`). Si cada una tiene su copia, las
reglas divergen en silencio y un cambio de provider pierde instrucciones. Con
una sola fuente en markdown, cambiar o sumar herramientas es agregar un
adaptador de una línea.

## Origen

Diagnóstico de 2026-10-28: un agente en omnigraf creó reglas Cursor con
`alwaysApply: true` que solo veía Cursor, y el puntero `AGENTS.MD` (mayúsculas)
fallaba en silencio en Linux. Ver `~/own/multi-project-router/BACKLOG.md`.
