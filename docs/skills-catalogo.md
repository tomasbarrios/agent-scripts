# Catálogo de skills — portabilidad e instalación

## Por qué existe

`skills/` en este repo es la **fuente de verdad única** de mis skills
propias, versionadas en git. Este catálogo dice, para cada una, si es
**agnóstica de cliente** (Claude Code, Hermes, Cursor, bb, o el que venga) o si asume
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
| `orchestrate` | Orquesta una tarea grande dividiéndola en plan (modelo caro) + ejecución delegada en paralelo (modelo barato). Agnóstico de dominio. | Subagentes/threads paralelos — modelo caro Opus 4.8 o Fable 5, modelo barato Sonnet 5 o Haiku 4.5 (se elige el que esté disponible). | ✅ agnóstica, lista tal cual — el patrón se elige por familia de modelo, no por nombre fijo |
| `product-designer-narrativa` | Genera narrativas breves para motivar flujos de conversión desde psicología humana y diseño de producto. | Ninguna específica — puro razonamiento | ✅ agnóstica, lista tal cual |
| `grilling` | Entrevista relentless con **rondas por frontera** (solo pregunta lo que ya tiene prerequisitos resueltos) y delega hechos a sub-agentes en vez de preguntarlos. Versión más elaborada de `interview-me`. | Ninguna específica — sub-agentes para buscar hechos. Externa (mattpocock), ver procedencia en su `SKILL.md`. | ✅ agnóstica, lista tal cual |
| `domain-modeling` | Construye y afila el modelo de dominio de un proyecto (glosario, ADRs) mientras se diseña. | Ninguna específica — escribe `CONTEXT.md`/ADRs en el repo. Externa (mattpocock), ver procedencia en su `SKILL.md`. | ✅ agnóstica, lista tal cual |
| `grill-with-docs` | `grilling` + `domain-modeling` combinadas: entrevista relentless que además deja ADRs y glosario. | Depende de `grilling` y `domain-modeling` (ambas en este repo). Externa (mattpocock), ver procedencia en su `SKILL.md`. | ✅ agnóstica, lista tal cual (se invoca a mano: `disable-model-invocation`) |
| `product-description` | Arma un **repo de spec de comportamiento** outside-in (feature a feature, event a event) leyendo código y tests, lo verifica contra el producto vivo y junta lo que se ve mal en un triage. El output es otro directorio/repo, no una skill. | Ninguna específica — escribe markdown; verificación usa el producto corriendo (browser, CLI, etc.). Externa (steveruizok), ver procedencia en su `SKILL.md`. | ✅ agnóstica, lista tal cual (se invoca a mano: `disable-model-invocation`) |
| `asb-who-me` | Interroga al usuario para descubrir quién es de verdad (qué lo mueve, qué lo drena, fortalezas invisibles); deja `WHO-ME.md`. Paso 1 del workshop **Find Yourself**. | Ninguna específica — puro razonamiento + escribe markdown. Externa (asmartbear), ver procedencia en su `SKILL.md`. | ✅ agnóstica, lista tal cual |
| `asb-voters` | Destila fortalezas a los uno o dos **voters** decisivos; deja `VOTERS.md`. Paso 2 de **Find Yourself**, consume el output de `asb-who-me`. | Ninguna específica — puro razonamiento + escribe markdown. Externa (asmartbear), ver procedencia en su `SKILL.md`. | ✅ agnóstica, lista tal cual |
| `asb-problem` | Puntúa si una idea puede volverse negocio viable (Fermi-score de un mercado sobre siete criterios que multiplican). | Ninguna específica — puro razonamiento. Externa (asmartbear), ver procedencia en su `SKILL.md`. | ✅ agnóstica, lista tal cual |

Cuando se instala una skill nueva (propia o de una [fuente
externa](../rules/fuentes-externas-de-skills.md)), agregarla acá con su
fila.

**Nota aparte — plugins de Cursor:** `pstack` ([procedencia](../rules/fuentes-externas-de-skills.md))
no es una skill portable en el sentido de esta tabla: es un plugin completo
de Cursor (`skills/` + `agents/` + `automations/` + `.cursor-plugin/plugin.json`).
La copia (vendor) vive en `.cursor/plugins/pstack` de este repo. La instalación
que Cursor y bb descubren de verdad es user-scope en
`~/.cursor/plugins/local/pstack` (hardlinks de archivos; Cursor ignora un
symlink que apunte fuera de `local/`). En bb aparece como plugin
`acp-cursor` (`pstack:poteto-mode`, etc.); bb carga skills al spawn de un
thread nuevo. No se copia a `~/.bb/skills/` ni se symlinkea a Claude/Hermes.

## Habilitar en un cliente

Cada cliente lee **solo** su propio directorio. Instalar en Cursor o Claude
Code no llega a bb (ni al revés).

Destinos actuales:

| Cliente | Directorio | Cómo instalar |
|---------|------------|---------------|
| Claude Code | `~/.claude/skills/` | symlink del directorio |
| Hermes | `~/.hermes/skills/` | symlink del directorio |
| Cursor | `~/.cursor/skills/` | symlink del directorio |
| bb | `~/.bb/skills/` | **no symlink** — ver abajo |

Claude, Hermes y Cursor:

```bash
SRC=/home/tomas/Projects/agent-scripts/skills
for DST in ~/.claude/skills ~/.hermes/skills ~/.cursor/skills; do
  mkdir -p "$DST"
  ln -s "$SRC/<nombre>" "$DST/<nombre>"
done
```

bb **rechaza** un directorio de skill que sea symlink (y cualquier symlink
dentro del árbol). Por eso no aparece en Extensions → Skills ni como slash
command, aunque el mismo `SKILL.md` esté bien en Cursor. El directorio en
`~/.bb/skills/<nombre>/` tiene que ser real; los archivos se hardlinkean
al repo (mismo inodo, se edita en un lado):

```bash
SRC=/home/tomas/Projects/agent-scripts/skills
DST=~/.bb/skills/<nombre>
rm -rf "$DST"
mkdir -p "$DST"
cp -al "$SRC/<nombre>/." "$DST/"
```

`cp -al` exige mismo filesystem. Para quitar: `rm` del symlink en Claude/
Hermes/Cursor, `rm -rf` del directorio en bb (el hardlink no borra el
archivo del repo). bb carga skills al **spawn de un thread nuevo**.

## Estado real instalado (verificado 2026-09-02, bb con directorios reales)

A diferencia del índice de arriba (qué existe y es portable), esta tabla
dice qué hay **de verdad** en cada cliente ahora mismo. Se desincroniza
con el tiempo — verificar en disco antes de confiar en ella, no asumir.
En bb la marca significa directorio real (hardlinks), no symlink.

| Skill | `~/.claude/skills/` | `~/.hermes/skills/` | `~/.cursor/skills/` | `~/.bb/skills/` |
|-------|:---:|:---:|:---:|:---:|
| `interview-me` | ✅ | ✅ | — no symlinkeada | ✅ |
| `idea-refine` | ✅ | ✅ | — no symlinkeada | ✅ |
| `product-expert-definition-of-ready` | ✅ | ✅ | — no symlinkeada | ✅ |
| `product-designer-narrativa` | ✅ | ✅ | ✅ | ✅ |
| `css-debug-explain-solve-and-suggest` | ✅ | ✅ | ✅ | ✅ |
| `voz-tomas` | — no symlinkeada | — no symlinkeada | — no symlinkeada | — no symlinkeada |
| `speech-to-text` | — no symlinkeada | — no symlinkeada | — no symlinkeada | — no symlinkeada |
| `orchestrate` | ✅ | ✅ | ✅ | ✅ |
| `wayfinder` | ✅ | ✅ | ✅ | ✅ |
| `grilling` | ✅ | ✅ | ✅ | ✅ |
| `domain-modeling` | ✅ | ✅ | ✅ | ✅ |
| `grill-with-docs` | ✅ | ✅ | ✅ | ✅ |
| `product-description` | ✅ | ✅ | ✅ | ✅ |
| `asb-who-me` | ✅ | ✅ | ✅ | ✅ |
| `asb-voters` | ✅ | ✅ | ✅ | ✅ |
| `asb-problem` | ✅ | ✅ | ✅ | ✅ |
| `scratch-review` | — no instalada | — no instalada | — no instalada | ✅ |

## Verificación

```bash
ls -la ~/.claude/skills ~/.hermes/skills ~/.cursor/skills ~/.bb/skills
bb skill list   # las de bb tienen que salir con scope bb-user; un
                # symlink en ~/.bb/skills no aparece acá
```

Hermes además: `hermes skills list`. Si no aparecen, el cliente puede
requerir reinicio de sesión o registro en su manifest; avisar y revisar.
