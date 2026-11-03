# Vendor: mattpocock / skills

Skills de ingeniería «real» (planificación, revisión, TDD, diagnóstico de
bugs, escritura para agentes) pensadas para adaptarse a cada repo. Es un
catálogo grande y organizado por categorías.

## Procedencia

| Campo | Valor |
|---|---|
| Repo | <https://github.com/mattpocock/skills> |
| Acceso | Submódulo `vendor/mattpocock-skills` (dentro de este repo) |
| Fecha de alta | previa (ver historia de `.gitmodules`) |

## Estructura del repo

Las skills viven en `skills/<categoría>/<name>/SKILL.md`, agrupadas en:

- **engineering** — `ask-matt` (router), `codebase-design`, `code-review`,
  `diagnosing-bugs`, `domain-modeling`, `grill-with-docs`, `implement`,
  `improve-codebase-architecture`, `prototype`, `research`,
  `resolving-merge-conflicts`, `setup-matt-pocock-skills`, `tdd`, `to-spec`,
  `to-tickets`, `triage`, `wayfinder`, `wizard`.
- **productivity** — `grilling`, `grill-me`, `handoff`, `teach`,
  `to-questionnaire`, `wait-what`, `writing-for-agents`.
- **misc** — `git-guardrails-claude-code`, `migrate-to-shoehorn`,
  `scaffold-exercises`, `setup-pre-commit`.
- **in-progress** — `claude-handoff`, `loop-me`, `setup-ts-deep-modules`,
  `writing-beats`, `writing-fragments`, `writing-shape` (inestables).
- **deprecated** — no usar.

## Qué ya se instaló desde acá

Estas se copiaron/adaptaron a `skills/` de este repo (ver procedencia en cada
`SKILL.md` y su fila en [`../skills-catalogo.md`](../skills-catalogo.md)):

- `wayfinder` — planifica trabajo grande como un mapa de tickets de decisión.
- `grilling` — entrevista relentless por rondas de frontera.
- `domain-modeling` — construye glosario/ADRs del dominio mientras se diseña.
- `grill-with-docs` — `grilling` + `domain-modeling` combinadas.

## Actualizar el submódulo

```bash
git submodule update --remote vendor/mattpocock-skills
```

Deja el bump de commit en un commit propio.
