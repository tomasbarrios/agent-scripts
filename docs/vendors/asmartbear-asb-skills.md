# Vendor: asmartbear / asb-skills

Skills públicas de Claude Code que implementan los frameworks de
[Jason Cohen](https://longform.asmartbear.com) (A Smart Bear) y del libro
*[Hidden Multipliers](https://hiddenmultipliers.com)*. Todas son de estrategia
de producto/negocio: cliente ideal, entrevistas de descubrimiento, pricing,
positioning. Ninguna toca código.

## Procedencia

| Campo | Valor |
|---|---|
| Repo | <https://github.com/asmartbear/asb-skills> |
| Sitio | <https://skills.asmartbear.com> |
| Acceso | Submódulo `vendor/asmartbear-asb-skills` (dentro de este repo) |
| Commit fijado | `740fe85` |
| Fecha de alta | 2026-08-26 |
| Licencia | Contenido de skills: CC BY 4.0 (atribución a Jason Cohen) |

## Estructura del repo

- `.claude/skills/asb-<name>/SKILL.md` — las **skills públicas** (21). Cada una
  es un único markdown autocontenido y agnóstico de cliente. Es la fuente de
  verdad; el sitio, el ZIP, la CLI `skills.sh` y el marketplace de plugin
  sirven estos mismos archivos.
- `dev-skills/<name>/` — tooling **interno** del repo (sin prefijo `asb-`,
  `metadata.internal: true`), nunca se distribuye. Son 5: `create-asb-skill`,
  `exercise-asb-skill`, `doc-skills`, `book-prose-writer`,
  `jason-corpus-search`. No las contamos como oferta.
- `.claude-plugin/marketplace.json`, `skills.sh.json`, `plugins/` —
  artefactos de distribución generados (`bun run gen-manifests`), no editar.

## Skills que ofrece (21 públicas)

Vienen organizadas en cuatro **workshops** (secuencias que se corren en orden)
más cuatro skills sueltas. El orden es el del propio repo.

### Workshop «Find Yourself» — quién sos vos, antes de la estrategia

| Skill | Qué hace |
|---|---|
| `asb-who-me` | Interroga al usuario para descubrir quién es de verdad: qué lo mueve, qué lo drena y las fortalezas naturales que no ve porque le salen fáciles. Deja `WHO-ME.md`. |
| `asb-voters` | Destila esas fortalezas a los uno o dos **voters**: los rasgos idiosincráticamente extremos que ganan el contrato porque el cliente que los valora acepta todos los demás trade-offs. Deja `VOTERS.md`. |

### Workshop «Find Your Carol» — definir el cliente ideal (ICP), paso a paso

| Skill | Qué hace |
|---|---|
| `asb-carol-observations` | Paso 1: junta observaciones crudas, honestas y específicas sobre qué es la empresa/producto, antes de todo juicio. Deja `OBSERVATIONS.md`. |
| `asb-carol-strengths` | Paso 2: destila las observaciones en atributos de fondo y clasifica cada uno como fortaleza, debilidad o ambos. Deja `STRENGTHS-WEAKNESSES.md`. |
| `asb-carol-keystones` | Paso 3: refina las fortalezas en **keystones** — los rasgos que hacen que un cliente NECESITE una versión extrema de una fortaleza. Deja `KEYSTONES.md`. |
| `asb-carol-dealbreakers` | Paso 4: convierte debilidades en deal-breakers (lo que descalifica al producto de plano) y mapea el anti-mercado. Deja `DEALBREAKERS.md`. |
| `asb-carol-inciting-events` | Paso 5: mapea los eventos que gatillan la compra ahora (no algún día). Deja `INCITING-EVENTS.md`. |
| `asb-carol-define` | Paso final: sintetiza todo en `CAROL.md`, la definición nítida del cliente ideal (conductual, no demográfica). |

### Workshop «Customer Interviews» — descubrimiento con método de hipótesis iterativas

| Skill | Qué hace |
|---|---|
| `asb-interview-goals` | Paso 1: define qué querés aprender, como preguntas-objetivo numeradas (G1, G2…). Deja `GOALS.md`. |
| `asb-interview-hypotheses` | Paso 2: registra tus mejores conjeturas como hipótesis falsables (H1, H2…) mapeadas a los objetivos. Deja `HYPOTHESES.md`. |
| `asb-interview-questions` | Paso 3: traduce hipótesis en preguntas abiertas y sin sesgo (Q1, Q2…). Deja `QUESTIONS.md`. |
| `asb-interview-debrief` | Aprendizaje (registro): convierte UNA conversación en un debrief por persona mapeado contra las preguntas. |
| `asb-interview-learning` | Aprendizaje (síntesis): lee los debriefs contra las hipótesis y propone actualizaciones citando evidencia; veredicto seguir/parar/re-apuntar. |
| `asb-interview-report` | Paso final: destila todo en `FINAL-REPORT.md` con hallazgos numerados y briefs por área. |

### Workshop «Set Your Price» — construir el valor detrás del precio y cobrarlo

| Skill | Qué hace |
|---|---|
| `asb-more-or-less` | Fuerza elegir UNA de las tres estrategias de pricing auto-consistentes (More for More / More for Less / Less for Less) y alinear la empresa detrás. |
| `asb-willingness-to-pay` | Mapea el deseo detrás de por qué pagan (Love / Utility / Coercion) y compromete a cuál valen la pena tener — sin tocar el número del precio. |
| `asb-raise-prices` | Arma el caso para subir precios y las jugadas que lo justifican; empuja hacia un precio más alto (no debate si subir). |

### Skills sueltas (fuera de workshop)

| Skill | Qué hace |
|---|---|
| `asb-rude-qa` | Interroga una decisión/plan/pitch con preguntas filosas, hasta hostiles, hasta afilarlo en decisiones defendibles o concluir que no era buena idea. El abogado del diablo constructivo. |
| `asb-problem` | Puntúa si una idea puede volverse negocio viable: Fermi-score de un mercado objetivo sobre siete criterios que multiplican (Plausible, Self-Aware, Lucrative, Liquid, Eager ×2, Enduring). |
| `asb-positioning` | Convierte copy de marketing en lenguaje value-first y vívido; ajusta cada claim al nivel correcto del needs stack y a la estrategia de pricing declarada. Deja `POSITIONING.md`. |
| `asb-needs-stack` | Construye el **Needs Stack** del cliente — la escalera donde cada necesidad es medio para la de arriba — y marca en qué nivel opera el producto. Deja `NEEDS-STACK.md`. |

## Qué ya se instaló desde acá

Copiadas a `skills/` de este repo (modo vendor, con procedencia en cada
`SKILL.md`) y symlinkeadas en `~/.claude`, `~/.hermes`, `~/.cursor` y
`~/.bb` — ver su fila en [`../skills-catalogo.md`](../skills-catalogo.md):

- `asb-who-me` y `asb-voters` — el workshop **Find Yourself** completo.
- `asb-problem` — scoring de viabilidad de una idea.

El resto (16) sigue como biblioteca; instalar cualquiera necesita aprobación.

## Cómo se instalaría el resto (si se aprueba)

Es biblioteca. Opciones upstream:

- **CLI skills.sh:** `npx skills add asmartbear/asb-skills` (todas) o
  `--skill asb-positioning` (una); `npx skills update` para avanzar.
- **Plugin de Claude Code:** `/plugin marketplace add asmartbear/asb-skills`
  y `/plugin install asb-skills@asb-skills`.
- **A mano (modo vendor de este repo):** copiar
  `.claude/skills/asb-<name>/SKILL.md` a `skills/asb-<name>/`, dejando
  procedencia, y registrarla en [`../skills-catalogo.md`](../skills-catalogo.md).

La instalación de cualquiera de estas requiere **aprobación explícita** de
Tomás — ver [`../../rules/fuentes-externas-de-skills.md`](../../rules/fuentes-externas-de-skills.md).

## Actualizar el submódulo

```bash
git submodule update --remote vendor/asmartbear-asb-skills
```

Deja el bump de commit en un commit propio (el submódulo fija un SHA, no se
mueve solo).
