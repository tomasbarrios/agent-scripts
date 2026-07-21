# Reglas globales (todos mis repos)

Este archivo se lee antes que nada en cualquier repo mío, vía la línea:
`READ ~/Projects/agent-scripts/AGENTS.MD BEFORE ANYTHING (skip if missing).`

## A reforzar (peso alto)

Leer [`rules/reforzar.md`](rules/reforzar.md) y aplicarlo activamente: reglas que Tomás
quiere adquirir — corregirlo cuando no las siga. Hoy: escritura breve.

## Reglas

- **Alcance disciplinado:** solo lo aprobado; sin infraestructura
  especulativa; no salir del repo sin aprobación.
  Fuente: [`rules/alcance-disciplinado.md`](rules/alcance-disciplinado.md).
- **Comunicación pedagógica:** explicar el porqué; humano primero, técnico
  después; Antes/Después/Porqué.
  Fuente: [`rules/comunicacion-pedagogica.md`](rules/comunicacion-pedagogica.md).
- **Declaración vs explicación:** al crear reglas/skills/hooks, declaración
  corta en el archivo de la herramienta, explicación en doc visible.
  Fuente: [`rules/declaracion-vs-explicacion.md`](rules/declaracion-vs-explicacion.md).
- **Aviso de brecha en skill:** si un skill no alcanza y creo un archivo
  efímero para completar la tarea, lo declaro — qué faltó y si el skill es
  global (se repite en otros repos).
  Fuente: [`rules/aviso-brecha-skill.md`](rules/aviso-brecha-skill.md).
- **Review estilo Tech Lead:** al revisar código, ADRs, planes o arquitectura,
  buscar el cambio mínimo que resuelva el problema real y enseñar el criterio
  detrás. Aplica solo cuando el usuario pida una revisión.
  Fuente: [`rules/techlead-review.md`](rules/techlead-review.md).

Las reglas específicas de cada proyecto viven en el AGENTS.md de ese repo,
después de la línea READ — incluidas las de este mismo repo (ver
[`docs/candidates.md`](docs/candidates.md) § Cómo promover).
