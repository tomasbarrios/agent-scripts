# Vendors — fuentes externas de skills (biblioteca)

Cada vendor es una **fuente externa** de skills que vive como submódulo git
en `vendor/`. Estas skills son **biblioteca, no están instaladas**: se buscan y
se proponen, pero no entran a `skills/` (la fuente de verdad de mis skills
propias) sin aprobación explícita — ver
[`../../rules/fuentes-externas-de-skills.md`](../../rules/fuentes-externas-de-skills.md).

Cada vendor tiene su página con las skills que ofrece.

| Vendor | Página | Qué aporta | Skills ofrecidas |
|---|---|---|---|
| asmartbear / asb-skills | [asmartbear-asb-skills.md](asmartbear-asb-skills.md) | Frameworks de estrategia de producto/negocio (cliente ideal, entrevistas, pricing, positioning) de Jason Cohen | 21 públicas |
| mattpocock / skills | [mattpocock-skills.md](mattpocock-skills.md) | Skills de ingeniería «real»: planificación, revisión, TDD, diagnóstico, escritura para agentes | ~30 (varias categorías) |
| steveruizok / product-description | [steveruizok-product-description.md](steveruizok-product-description.md) | Spec de comportamiento outside-in verificada contra el producto vivo | 1 (gist plano) |

Los clones **hermanos** (`../agent-scripts-stein`, `../agent-skills-addy`) no
son submódulos de este repo, así que no tienen página acá — se documentan en la
tabla de fuentes de la regla.

## Diferencia con el catálogo de skills

- [`../skills-catalogo.md`](../skills-catalogo.md) = qué skills **propias**
  están instaladas en `skills/` y dónde symlinkeadas. Fuente de verdad de lo
  instalado.
- Estas páginas de vendor = qué ofrece cada fuente **externa** upstream. No
  duplican el catálogo; cuando algo se instala desde un vendor, se registra en
  el catálogo y se cruza desde acá.

## Agregar un vendor nuevo

1. `git submodule add <url> vendor/<owner>-<repo>`.
2. Analizar el repo y crear su página acá (procedencia con repo, commit fijado
   y fecha; estructura; tabla de skills ofrecidas).
3. Agregar su fila en esta tabla y en la tabla de fuentes de
   [`../../rules/fuentes-externas-de-skills.md`](../../rules/fuentes-externas-de-skills.md).
