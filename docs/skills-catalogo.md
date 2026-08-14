# Catálogo de skills — portabilidad e instalación

## Por qué existe

`skills/` en este repo es la **fuente de verdad única** de mis skills
propias, versionadas en git. Este catálogo dice, para cada una, si es
**agnóstica de cliente** (Claude Code, Hermes, o el que venga) o si asume
herramientas/tools específicas — para decidir rápido si es instalable en
otro lado tal cual o si necesita adaptación.

## Índice de skills disponibles

| Skill | Qué hace | Herramienta que asume | Portabilidad |
|-------|----------|----------------------|--------------|
| `interview-me` | Entrevista de **una pregunta a la vez** con una hipótesis adjunta, hasta ~95% de confianza sobre lo que querés de verdad (no lo que creés que deberías querer). | `AskUserQuestion` (Claude Code) → en Hermes usamos `clarify`. | ⚠️ el método sirve; el paso de preguntas hay que mapearlo a la tool equivalente del cliente |
| `idea-refine` | Refina una idea vaga en concepto accionable (divergente → convergente), con sesión de ideación y un one-pager. | `AskUserQuestion`, `Glob`/`Grep`/`Read` | ⚠️ parcial — las preguntas van por la tool de clarificación del cliente |
| `product-expert-definition-of-ready` | Evalúa un cambio de producto en 4 dimensiones (narrativa, conductual, copy, framing) ponderadas por momento del journey. | Ninguna específica — puro razonamiento + plantilla en `assets/` | ✅ agnóstica, lista tal cual |
| `voz-tomas` | Ajusta tono/vocabulario para que un texto suene a como habla Tomás (no define narrativa ni contenido). | Ninguna específica — puro razonamiento + `ejemplos.md` para aprender por casos | ✅ agnóstica, lista tal cual |
| `css-debug-explain-solve-and-suggest` | Diagnostica y resuelve bugs de CSS/layout en 4 fases (causa raíz → explicación → intervención mínima → mejores opciones). | Ninguna específica — puro razonamiento + navegación del código | ✅ agnóstica, lista tal cual |
| `speech-to-text` | Transcribe un archivo de audio de conversación; pregunta primero si hace falta distinguir hablantes (diarización). | `whisper`; `whisperx` + `HF_TOKEN` si hay diarización | ⚠️ requiere `whisper`/`whisperx` disponibles en el entorno destino |
| `wayfinder` | Planifica un trabajo grande que no cabe en una sesión: un "mapa" con tickets de **decisión** que se resuelven de a uno hasta que el camino está claro. Planifica, no ejecuta. | Ninguna específica — escribe markdown en `.scratch/` del repo. Externa (mattpocock), ver procedencia en su `SKILL.md`. | ✅ agnóstica, lista tal cual (se invoca a mano: `disable-model-invocation`) |
| `orchestrate` | Orquesta una tarea grande dividiéndola en plan (modelo caro) + ejecución delegada en paralelo (modelo barato). Agnóstico de dominio. | Subagentes/threads paralelos — nombres de modelo hardcodeados (Fable 5 / Sonnet 5). | ⚠️ el patrón sirve; los nombres de modelo hay que ajustarlos si el cliente no tiene esos mismos modelos disponibles |
| `product-designer-narrativa` | Genera narrativas breves para motivar flujos de conversión desde psicología humana y diseño de producto. | Ninguna específica — puro razonamiento | ✅ agnóstica, lista tal cual |

Cuando se instala una skill nueva (propia o de una [fuente
externa](../rules/fuentes-externas-de-skills.md)), agregarla acá con su
fila.

## Habilitar en un cliente vía symlink

Estrategia general (la misma que usa steipete para cualquier cliente): el
directorio de skills del cliente apunta acá por symlink **absoluto** — así
editás en un lado y se refleja en el otro, sin desincronizar.

Ejemplo para Hermes:

```bash
SRC=/home/tomas/Projects/agent-scripts/skills
DST=/home/tomas/.hermes/skills
for s in interview-me idea-refine product-expert-definition-of-ready voz-tomas css-debug-explain-solve-and-suggest speech-to-text wayfinder orchestrate product-designer-narrativa; do
  ln -s "$SRC/$s" "$DST/$s"
done
```

Para quitar una: `rm $DST/<nombre>`. Para otro cliente, mismo patrón con su
propio directorio de skills como `DST`.

## Verificación (ejemplo Hermes)

```bash
hermes skills list | grep -E "interview-me|idea-refine|product-expert|voz-tomas|css-debug|speech-to-text|wayfinder|orchestrate|product-designer"
```

Si no aparecen, el cliente puede requerir reinicio de sesión o registro en
su manifest de skills; avisar y revisar.
