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
- **Autoría de ideas:** no agregar ideas propias a una lista del usuario
  sin preguntar antes; cuando conviven, marcar siempre quién aporta cada
  una (`Tomás` / `agente`).
  Fuente: [`rules/autoria-de-ideas.md`](rules/autoria-de-ideas.md).
- **Escalera de exploración:** ante un objetivo **ambicioso** de forma
  incierta, no construir directo: proponer una escalera de escalones chicos
  que componen sobre un cimiento, cada uno con su pregunta de feedback, y
  **consultarla antes de construir**. No aplica a trabajo de forma conocida.
  Fuente: [`rules/escalera-de-exploracion.md`](rules/escalera-de-exploracion.md).
- **Review estilo Tech Lead:** al revisar código, ADRs, planes o arquitectura,
  buscar el cambio mínimo, cuidar entendibilidad (sweet spot vía trade-offs)
  y enseñar el criterio — con ejemplos de a dónde se mueve la complejidad.
  Aplica solo cuando el usuario pida una revisión.
  Fuente: [`rules/techlead-review.md`](rules/techlead-review.md).
- **Reglas globales → agent-scripts:** si una regla o método debe aplicar a
  más de un repo, **consultar** antes de crearla o modificarla aquí (no
  cerrarla solo en el repo de producto). Incluye cambios de taxonomía en
  `docs-kit/` (bump VERSION, luego migran consumidores).
  Fuente: [`rules/reglas-globales.md`](rules/reglas-globales.md).

Las reglas específicas de cada proyecto viven en el AGENTS.md de ese repo,
después de la línea READ — incluidas las de este mismo repo (ver
[`docs/candidates.md`](docs/candidates.md) § Cómo promover).
