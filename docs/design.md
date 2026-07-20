# Diseño: Sistema personal de skills y agents centralizados

> Documento de diseño agnóstico. Objetivo: describir el problema, el terreno y las
> decisiones pendientes con suficiente detalle como para que, ampliándolo, se le
> pueda inyectar a un agente y obtener una propuesta de solución completa.
> **No contiene decisiones tomadas** — donde hay una elección, se deja marcada
> como pregunta abierta.

## 1. Problema

Tengo skills y reglas de agente dispersas y duplicadas entre proyectos. Quiero
un repo personal central que sea la fuente de verdad, y un mecanismo para que
esa fuente de verdad llegue "al día" a cualquier proyecto en el que trabaje,
sin importar qué herramienta de agente use en ese proyecto (Claude Code,
Cursor, OpenCode, potencialmente otras a futuro).

Casos que el sistema debe cubrir:

1. **La skill existe en otro repositorio mío o de terceros** → quiero una
   copia local, idealmente sincronizada, pero sync no es un requisito duro.
2. **La skill no existe** → quiero crearla desde cero en mi repo central.
3. **La skill existe pero no me sirve tal cual** → quiero adaptarla
   (fork/override) sin perder la trazabilidad de dónde vino.

Además, quiero que el repo central se haga cargo de **agents** (personas /
subagentes / reglas de comportamiento), no solo de skills.

## 2. Terminología y mapeo de conceptos entre herramientas

Cada herramienta usa nombres distintos para conceptos parecidos. Antes de
diseñar nada hay que fijar el vocabulario y a qué corresponde en cada una.

| Concepto genérico | Claude Code | Cursor | OpenCode |
|---|---|---|---|
| Unidad de conocimiento/procedimiento reusable, invocable bajo demanda | **Skill** — `SKILL.md` con frontmatter `name` + `description` | **Skill** — Cursor lee nativamente `.cursor/skills/<name>/SKILL.md` (mismo formato, agent-discovered por `description`) | **Skill** — `SKILL.md` con frontmatter `name` + `description`, cargado on-demand vía tool nativo `skill` |
| Instrucciones que siempre aplican / reglas duras | `CLAUDE.md` (proyecto o `~/.claude/CLAUDE.md` global) | `.cursor/rules/*.mdc` con `alwaysApply: true`, o `AGENTS.md` plano | `AGENTS.md` (no confirmado en docs oficiales, pero es el estándar de facto que otras herramientas leen) |
| Persona / subagente con prompt y permisos propios | `.claude/agents/*.md` (frontmatter: `name`, `description`, `tools`, `model`) | No tiene subagentes nativos declarativos | `.opencode/agents/*.md` o bloque `agent` en `opencode.json` (frontmatter: `description`, `mode: primary\|subagent\|all`, `permission`, `model`) |
| Ubicación **global** (todos los proyectos de la máquina) | `~/.claude/skills/`, `~/.claude/agents/`, `~/.claude/CLAUDE.md` | User Rules (vía Settings, no archivo de proyecto) / Team Rules (Enterprise) | `~/.config/opencode/skills/`, `~/.config/opencode/agents/` |
| Ubicación **de proyecto** | `.claude/skills/`, `.claude/agents/`, `CLAUDE.md` | `.cursor/rules/*.mdc` | `.opencode/skills/`, `.opencode/agents/` (también lee `.claude/skills/` y `.agents/skills/` por compatibilidad) |
| Comando explícito invocado por el usuario (slash command) | `.claude/commands/*.md` | No hay equivalente directo | No confirmado |

**Dato clave para el diseño:** las tres herramientas convergen en el mismo
formato de origen — `SKILL.md` con frontmatter `name` + `description`. OpenCode
además lee directamente `.claude/skills/` y `.agents/skills/` sin necesitar
carpeta propia, y Cursor lee nativamente `.cursor/skills/<name>/SKILL.md`. La
fricción real no está en el formato (es prácticamente idéntico en las tres),
sino en **la ruta física** donde cada herramienta espera encontrarlo:
`.claude/skills/`, `.cursor/skills/`, `.opencode/skills/` (o sus variantes
globales). Esto simplifica bastante el problema: es más una cuestión de
"espejar una carpeta en tres ubicaciones" que de "traducir un formato a tres
formatos distintos".

## 3. Formato SKILL.md (denominador común)

Frontmatter mínimo, igual en Claude Code y OpenCode:

```yaml
---
name: kebab-case, debe matchear el nombre del directorio
description: corto, optimizado para *routing* (cuándo usarla), no para documentación
---
```

Cuerpo libre en markdown. En la práctica (ver `idea-refine` en este mismo
repo, y el `skill-anatomy.md` de Addy) suele incluir: overview, cuándo usarla,
pasos del proceso, anti-patrones / red flags, criterios de verificación, y
archivos de referencia auxiliares en la misma carpeta (`scripts/`,
`references/`, etc.) que la skill lee bajo demanda.

Esto sugiere que el **formato de origen** en el repo central puede ser
`SKILL.md` puro, sin adaptar, y que la fricción real está en la
**distribución**: cómo ese mismo archivo llega a la ruta que cada herramienta
espera en cada proyecto.

## 4. Los tres patrones de distribución observados

Estudiando referencias existentes (steipete/agent-scripts, addyosmani/agent-skills)
aparecen tres estrategias distintas para "llevar" contenido desde un repo
central a `~/.claude/...` o `.claude/...` en cada proyecto:

1. **Symlink tracked en git.** El repo central tiene la skill real en una
   ubicación canónica (en este repo: `skills/<nombre>/`; en otro repo podría
   ser p.ej. `mi-otro-repo/.agents/skills/foo`) y el consumidor apunta ahí
   con un symlink. Sin duplicación de contenido, pero requiere que ambos
   repos estén clonados en el mismo filesystem con rutas relativas estables.
2. **Script de sync/install idempotente.** Un script (`sync-skills`) recorre
   el repo central y crea/actualiza symlinks o copias en
   `~/.claude/skills/`, `~/.codex/skills/`, etc. Corre manualmente o por hook.
   Resuelve colisiones por precedencia declarada.
3. **CLI/paquete instalable.** (`npx skills add usuario/repo`) — trata las
   skills como un paquete que se instala por proyecto, con soporte para
   múltiples plataformas objetivo vía manifests (`.claude-plugin/`,
   `.codex-plugin/`, etc.).

Ninguno resuelve "sync en caliente" de forma automática sin intervención —
todos requieren correr algo (un script o un comando) para actualizar. Esto es
consistente con el requisito del usuario de que sync "no es requisito" pero
sí "al día" bajo demanda.

## 5. Casos de uso, resueltos caso por caso

En vez de preguntas abstractas, esta sección recorre situaciones concretas
del día a día y muestra, para cada una, **cómo la resuelve cada repo de
referencia**. Ninguna fila es una decisión tomada — es un relevamiento para
comparar mecanismos antes de elegir.

### 5.1 ¿Cómo instalo una skill por primera vez en un proyecto?

| Repo | Mecanismo | Resultado en el proyecto destino |
|---|---|---|
| **steipete/agent-scripts** | Clonás el repo central una vez en la máquina y corrés `scripts/sync-skills`. No hay paso "por proyecto": el script crea/actualiza symlinks a nivel de **usuario** (`~/.claude/skills/`, `~/.codex/skills/`). | Todo proyecto en esa máquina ve las skills automáticamente vía la config global de Claude Code / Codex — no hay nada que instalar dentro del repo de proyecto. |
| **addyosmani/agent-skills** | `npx skills add addyosmani/agent-skills` corrido **dentro del proyecto** (usa el CLI `vercel-labs/skills`). Soporta `--symlink` (apunta a una copia canónica local) o copia real; por defecto instala en el scope del proyecto, con `-g` para scope global. | Crea `./.claude/skills/`, `./.cursor/skills/`, etc. **dentro de ese repo**, comiteados como config del proyecto. Hay que repetir el comando por cada proyecto (o usar `-g`). |

**Diferencia clave:** Peter resuelve "instalación" una vez a nivel de máquina
(global); Addy la resuelve por proyecto (con opción global). Esto determina
si un proyecto nuevo "hereda" las skills automáticamente o si necesita un
paso explícito.

### 5.2 ¿Cómo modifico localmente una skill instalada en un proyecto?

| Repo | Mecanismo | Consecuencia |
|---|---|---|
| **steipete/agent-scripts** | Como la instalación es un **symlink** hasta el archivo real del repo central, editar el archivo "instalado" **es editar el archivo central directamente** (mismo inodo). No hay copia local que diverja. | No existe el concepto de "edición local" — cualquier edición es, de hecho, una edición al repo central, visible en el próximo `git status` de ese repo. |
| **addyosmani/agent-skills (CLI copy mode)** | Si se instaló en modo copia (no symlink), el archivo en `.claude/skills/<name>/SKILL.md` es independiente del repo fuente. Editarlo es una edición local pura. | Cambia sin afectar a nadie más, pero un `skills update` posterior puede sobrescribirlo sin aviso (no hay merge documentado). |
| **addyosmani/agent-skills (CLI symlink mode) / Cursor via rsync** | Igual que Peter si se usó `--symlink`. Con `rsync` (patrón documentado para Cursor) es copia plana: `rsync -a origen/ .cursor/skills/` sobrescribe todo, `--ignore-existing` preserva ediciones locales pero también bloquea updates de esos archivos. | Editar y luego re-sincronizar sin `--ignore-existing` **pierde el cambio local** silenciosamente. |

**Diferencia clave:** con symlinks no hay "local" vs "central" — es un solo
archivo. Con copia (CLI en modo copy, o rsync), edición local y edición
central son cosas distintas, y el riesgo de pisar cambios locales al
actualizar depende de flags que hay que recordar usar.

### 5.3 ¿Cómo reflejo en el repo central un cambio que hice en el repo destino?

| Repo | Mecanismo |
|---|---|
| **steipete/agent-scripts** | Automático si es symlink: el cambio ya está en el repo central porque es el mismo archivo. Solo falta `git add`/`commit`/`push` **en el repo central** (no en el proyecto destino). |
| **addyosmani/agent-skills** | No documentado. En modo copia no hay mecanismo de "subir" el cambio — habría que copiar manualmente el archivo modificado de vuelta al repo `agent-skills` y commitear ahí. El CLI (`vercel-labs/skills`) no tiene comando de "push back" ni de diff contra el origen. |

**Diferencia clave:** este es el caso donde el enfoque de Peter gana con
claridad — al ser el mismo archivo, "reflejar cambios" no es una operación
distinta de "hacer el cambio". El enfoque de Addy requiere un paso manual no
resuelto por su tooling.

### 5.4 ¿Cómo traigo actualizaciones nuevas del repo central hacia un proyecto ya instalado?

| Repo | Mecanismo |
|---|---|
| **steipete/agent-scripts** | `git pull` en el repo central + re-correr `scripts/sync-skills` (idempotente, solo repara symlinks rotos/nuevos). Como es symlink, en rigor ni siquiera hace falta re-correr el script salvo que se hayan agregado/quitado skills — el contenido ya está al día apenas se hace `git pull`. |
| **addyosmani/agent-skills** | `npx skills update` (o re-`add`) dentro de cada proyecto. En modo copia esto sobrescribe el archivo local con la versión upstream — sin merge, sin preservar ediciones locales. |

### 5.5 ¿Cómo incorporo una skill que vive en otro repo mío (no en el repo central)?

| Repo | Mecanismo |
|---|---|
| **steipete/agent-scripts** | Symlink tracked en git desde el repo central hacia la ubicación canónica en el otro repo (ej. `skills/discrawl -> ../../discrawl/skills/discrawl`), asumiendo que ambos repos están clonados como hermanos en el filesystem con una ruta relativa estable. `sync-skills` los descubre igual que a las skills propias. |
| **addyosmani/agent-skills** | No es un caso contemplado en su modelo — está diseñado para ser instalado *desde* un repo de skills hacia proyectos consumidores, no para agregar fuentes externas dispersas hacia adentro del propio repo de skills. |

**Diferencia clave:** este es exactamente el caso 1 que planteaste al
principio ("la skill existe en otro repositorio y quiero una copia... en
sync"), y es el patrón que Peter cubre explícitamente y Addy no.

### 5.6 ¿Cómo se resuelven colisiones cuando dos fuentes definen una skill con el mismo nombre?

| Repo | Mecanismo |
|---|---|
| **steipete/agent-scripts** | Precedencia explícita y fija en el script: `agent-scripts > manager > codex-local`. La primera fuente que "reclama" el nombre gana; las siguientes se saltean con un log de diagnóstico. |
| **addyosmani/agent-skills** | No documentado — no hay noción de múltiples fuentes compitiendo por un nombre dentro de un mismo proyecto instalado. |

### 5.7 ¿Cómo se cubre esto para Cursor específicamente?

| Repo | Mecanismo |
|---|---|
| **steipete/agent-scripts** | No tiene soporte específico para Cursor documentado (foco en Codex y Claude Code). |
| **addyosmani/agent-skills** | Cursor lee nativamente `.cursor/skills/<name>/SKILL.md` (mismo formato que Claude/OpenCode). El flujo recomendado es `rsync -a agent-skills/skills/ .cursor/skills/` dentro del proyecto — es decir, copia plana, sin symlink, con el riesgo de sobrescritura descrito en 5.2. |

**Nota:** dado que Cursor usa el mismo formato `SKILL.md` que Claude Code y
OpenCode (ver sección 2), en principio el mecanismo de symlink de Peter
debería funcionar igual de bien para `.cursor/skills/` que para
`.claude/skills/` — Addy usa `rsync` probablemente por compatibilidad con
Windows/entornos sin symlinks, no porque Cursor lo requiera.

### 5.8 ¿Cómo se cubren los agents/subagentes (no skills)?

| Repo | Mecanismo |
|---|---|
| **steipete/agent-scripts** | No los trata como unidad instalable individual. Centraliza reglas de comportamiento en un único `AGENTS.MD` en la raíz del repo central, y cada proyecto downstream agrega **una línea puntero** ("leer `~/Projects/agent-scripts/AGENTS.MD` antes que nada") en su propio `AGENTS.md`/`CLAUDE.md` local. No hay traducción a `.claude/agents/*.md` ni a subagentes nativos de ninguna herramienta. |
| **addyosmani/agent-skills** | Sí los trata como unidad individual: carpeta `agents/` con personas predefinidas (`code-reviewer.md`, `security-auditor.md`, etc.), cada una instalable/copiable igual que una skill, con su propio frontmatter por herramienta. |

**Diferencia clave:** Peter evita mantener N formatos de subagente en
paralelo centralizando todo en texto plano referenciado por puntero; Addy
paga el costo de mantener personas estructuradas por herramienta a cambio de
poder invocarlas como subagentes nativos (`@code-reviewer` en Claude Code,
etc.).

### 5.9 ¿Cómo se valida que una skill nueva esté bien formada antes de commitear?

| Repo | Mecanismo |
|---|---|
| **steipete/agent-scripts** | `scripts/validate-skills` verifica que el frontmatter YAML (`name`, `description`) esté presente y bien formado en todos los `SKILL.md` del repo. Se corre manualmente o vía CI (`.github/workflows/`). |
| **addyosmani/agent-skills** | Define una plantilla estricta documentada en `docs/skill-anatomy.md` (secciones esperadas: overview, when-to-use, proceso, anti-rationalization, red flags, verificación), pero no queda claro si hay un validador automático além de convención documental. |

## 6. Lo que cada referencia aporta (sin decidir todavía cuál usar)

- **steipete/agent-scripts**: patrón simple y efectivo — symlinks tracked +
  script de sync idempotente + `AGENTS.md` único referenciado por puntero.
  Pocas piezas móviles, fácil de razonar, pero manual y sin plantilla/validación
  fuerte.
- **addyosmani/agent-skills**: patrón ordenado — plantilla estricta de
  `SKILL.md`, separación explícita `skills/` vs `agents/`, validación,
  soporte multi-herramienta vía manifests/build por plataforma, CLI de
  instalación. Más piezas móviles; el usuario ya reportó fricción para
  hacerlo funcionar en la práctica.

## 7. Criterios de éxito (borrador, a refinar)

- Puedo agregar una skill nueva en el repo central y usarla desde cualquier
  proyecto sin copiar/pegar contenido a mano.
- Un cambio en el repo central se refleja en todos los proyectos con, como
  máximo, un comando manual (no requiere editar cada proyecto uno por uno).
- Claude Code y OpenCode quedan cubiertos con la misma fuente sin
  duplicación de contenido.
- Cursor tiene al menos una vía definida (aunque sea parcial) para acceder al
  mismo conocimiento, aunque el mecanismo interno sea distinto.
- Puedo adaptar una skill a un proyecto específico sin romper la copia
  "canónica" para los demás proyectos.

## 8. No-objetivos (por ahora)

- No se busca publicar el repo como paquete/producto para terceros (a
  diferencia de Addy).
- No se busca soportar todas las herramientas de agentes existentes, solo
  Claude Code, Cursor y OpenCode como conjunto inicial.
- No se busca resolver sync en tiempo real (watch/push automático) — el
  requisito explícito es "no es requisito", así que no es un objetivo de
  esta primera versión.
