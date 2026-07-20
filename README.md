# agent-scripts

Repo central personal de reglas y skills para agentes (Claude Code, Cursor,
OpenCode). Fuente de verdad única; los proyectos apuntan acá.

## Dónde encontrar qué

- [`AGENTS.md`](AGENTS.md) — declaraciones para agentes que trabajan en este repo.
- [`rules/`](rules/) — reglas consolidadas, una por archivo. Índice en su README.
- [`rules/reforzar.md`](rules/reforzar.md) — reglas que quiero adquirir;
  **peso alto**: los agentes las aplican activamente y me corrigen.
- [`skills/`](skills/) — skills centralizadas (fuente de verdad); los
  consumidores las enlazan por symlink (`~/.cursor/skills/`, etc.).
  No usar `.agents/skills/` en este repo.
- [`docs/`](docs/) — proceso de construcción de este repo:
  [`candidates.md`](docs/candidates.md) (pipeline candidato → centralizado),
  [`plan-primeros-pasos.md`](docs/plan-primeros-pasos.md) (plan vigente),
  [`design.md`](docs/design.md) (diseño de la distribución).
- [`docs-kit/`](docs-kit/) — **método portable de organización de docs** (taxonomía
  carpeta ↔ pregunta). Copiar/clonar a otro proyecto u otra máquina: ver
  [`docs-kit/ADOPT.md`](docs-kit/ADOPT.md). Versión: [`docs-kit/VERSION`](docs-kit/VERSION).

## Ciclo de vida de una regla

candidato (`candidates.md`) → reforzar (`reforzar.md`, si es hábito nuevo) →
consolidada (`rules/`). Distribución (estrategia steipete): skills por
symlink; reglas vía [`AGENTS.md`](AGENTS.md) central, que cada repo lee con
una línea READ al inicio de su propio `AGENTS.md`.
