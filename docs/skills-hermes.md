# Skills centralizadas → Hermes (vía symlink)

## Por qué symlink y no copiar

Las skills viven como **fuente de verdad única** en `skills/` de este repo
(versionadas en git). En vez de copiarlas a `~/.hermes/skills/`, Hermes las
lee por symlink: editás en un lado, se refleja en el otro, y no se
desincronizan. Es la misma estrategia steipete que usan `~/.cursor/skills/`
y `~/.bb/skills/`.

El symlink debe ser **absoluto** (`/home/tomas/...`) para no romperse si se
ejecuta desde otro directorio.

## Índice de skills disponibles (elegí cuál invocar)

| Skill | Qué hace | Herramienta que asume | Lista para Hermes |
|-------|----------|----------------------|-------------------|
| `interview-me` | Entrevista de **una pregunta a la vez** con una hipótesis adjunta, hasta ~95% de confianza sobre lo que querés de verdad (no lo que creés que deberías querer). | `AskUserQuestion` (Claude Code) → en Hermes usamos `clarify`. | ⚠️ el método sirve; el paso de preguntas hay que hacerlo con `clarify` |
| `idea-refine` | Refina una idea vaga en concepto accionable (divergente → convergente), con sesión de ideación y un one-pager. | `AskUserQuestion`, `Glob`/`Grep`/`Read` | ⚠️ parcial — las preguntas van por `clarify` |
| `product-expert-definition-of-ready` | Evalúa un cambio de producto en 4 dimensiones (narrativa, conductual, copy, framing) ponderadas por momento del journey. | Ninguna específica — puro razonamiento + plantilla en `assets/` | ✅ lista tal cual |
| `orchestrate-opencode` | Plan en Sonnet, ejecución mecánica en modelos gratis de OpenCode vía CLI `bb`. | CLI `bb` + provider `acp-opencode` | ⚠️ requiere `bb` instalado y el provider disponible |

## Cómo habilitar (symlink absoluto)

```bash
SRC=/home/tomas/Projects/agent-scripts/skills
DST=/home/tomas/.hermes/skills
for s in interview-me idea-refine product-expert-definition-of-ready orchestrate-opencode; do
  ln -s "$SRC/$s" "$DST/$s"
done
```

Para quitar una: `rm /home/tomas/.hermes/skills/<nombre>`.

## Notas de portabilidad (agent-agnóstico)

- `interview-me` e `idea-refine` mencionan `AskUserQuestion` y herramientas de
  Claude Code. En Hermes la pregunta *one-at-a-time* se hace con la tool
  `clarify`; el resto del método es aplicable sin cambios.
- `orchestrate-opencode` depende del CLI `bb` y del provider `acp-opencode`;
  sin eso no ejecuta la fase de ejecución en paralelo.
- `product-expert-definition-of-ready` es agnóstico: solo razonamiento + la
  plantilla en `assets/plantilla.md` y la guía en `references/guia-explicativa.md`.

## Verificación

Después de crear los symlinks, confirmá que Hermes los ve:

```bash
hermes skills list | grep -E "interview-me|idea-refine|product-expert|orchestrate-opencode"
```

Si no aparecen, Hermes puede requerir reinicio de sesión o registro en el
manifest de skills del perfil; avisar y revisar.
