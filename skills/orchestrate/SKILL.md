---
name: orchestrate
description: "Orquestar cualquier tarea grande dividiéndola en plan + ejecución delegada: un modelo caro (Opus vía claude-code) piensa, diseña el plan y sintetiza; agentes baratos (Sonnet o Haiku, o de otro proveedor como Composer o Grok vía acp-cursor) ejecutan las partes en paralelo. Sirve para cualquier dominio — código, investigación, documentos, migraciones, contenido. Usar cuando el usuario pide 'orquestar', 'divide y ejecuta', 'usa agentes baratos', 'planifica y delega', o cuando una tarea tiene varias partes independientes que conviene paralelizar con costo mínimo."
---

# Orchestrate — Plan caro, ejecución barata

Patrón general de tres fases, agnóstico al dominio. La idea: el juicio y la descomposición valen tokens caros una sola vez; la ejecución mecánica no.

| Fase | Modelo | Por qué |
|------|--------|---------|
| Planning + Work Brief | Opus vía claude-code (thread actual) | La descomposición correcta es lo que más valor agrega |
| Ejecución de tareas | Sonnet/Haiku, o Composer/Grok vía acp-cursor (threads paralelos) | Trabajo mecánico spec-a-resultado — el modelo caro no agrega valor acá |
| Síntesis / revisión | Opus vía claude-code (thread actual) | Juntar resultados y juzgar calidad requiere criterio |

**Reasoning:** máximo `medium` en todos los threads spawneados (`--reasoning-level medium` o menor; si el modelo no lista `medium`, usá el nivel más alto que liste bajo ese tope).

**Ids de modelo:** no los escribas de memoria; sacalos de `bb provider models <provider>` (p. ej. `bb provider models claude-code`, `bb provider models acp-cursor`).

**Estado escrito:** mantené el estado (hecho / en curso / próximos pasos / ramas y hashes) escrito en el hilo o en un archivo de la tarea a lo largo del trabajo, no solo al final.

---

## Regla de proveedor cruzado

Antes de lanzar cada fase o hilo hijo, revisa el uso del proveedor y cuánto falta para que se renueve, y compara el uso con el tiempo que ya pasó de la ventana:

```bash
bb settings usage --refresh --json   # por proveedor: windows[].usedPercent y windows[].resetsAt
```

- **Normal:** si el uso va por debajo del tiempo transcurrido (por ejemplo, 30% de uso con el 50% de la ventana ya pasada), todo sigue en el proveedor principal.
- **Cruzado:** si el uso va por encima del tiempo transcurrido, o pasa del 50% y faltan más de 3 horas para la renovación, el principal solo planifica, revisa y sintetiza. Toda la ejecución se va a otro proveedor (Composer o Grok vía `acp-cursor`).
- **Reserva:** si pasa del 80%, el principal deja de trabajar en la tarea y todo sigue en otro proveedor. Antes de cortar, el principal deja escrito en el hilo el estado y los próximos pasos, para que otro modelo pueda retomar sin él.
- **Corte:** si un proveedor se queda sin límite o sin sesión, la tarea sigue de inmediato con otro proveedor desde ese estado escrito, sin esperar la renovación ni repetir lo que ya está hecho.
- Si no se puede saber el uso, se asume el modo cruzado.

---

## Phase 1 — Planning (quedate en el thread actual)

Trabajá iterativamente con el usuario hasta entender el objetivo. El entregable es un **Work Brief**: la tarea descompuesta en sub-tareas tan específicas que un agente barato pueda ejecutarlas sin adivinar nada.

Buenas sub-tareas son:
- **Independientes** — no dependen del resultado de otra sub-tarea (si hay dependencias, agrupalas en una sola o hacé rondas secuenciales)
- **Autocontenidas** — el prompt incluye todo el contexto necesario; el agente spawneado no ve esta conversación
- **Verificables** — el brief dice qué archivo/output produce cada una y cómo se ve "terminado"

### Work Brief format

```
## Work Brief

**Objetivo:** [una frase]
**Contexto compartido:** [lo que TODOS los agentes necesitan saber: rutas, convenciones, restricciones]

### Sub-tareas
1. [nombre] — [instrucción completa] — Output: [archivo/resultado esperado]
2. ...

### Criterio de éxito
- [cómo sabemos que el conjunto quedó bien]
```

Iterá hasta aprobación explícita del usuario ("listo", "dale", "procede", "ok sigamos"). No spawnees nada antes de eso, salvo que el usuario ya haya pedido ejecutar directo.

---

## Phase 2 — Ejecución (modelo barato, paralelo)

Spawná **un thread por sub-tarea**, todos en paralelo, con el proveedor que indique la [regla de proveedor cruzado](#regla-de-proveedor-cruzado). Modo normal (Haiku para lo mecánico/simple, Sonnet si requiere más criterio):

```bash
bb thread spawn \
  --project "$BB_PROJECT_ID" \
  --parent-self \
  --provider claude-code \
  --model <id de Sonnet o Haiku según bb provider models claude-code> \
  --reasoning-level medium \
  --prompt "[INSTRUCCIÓN COMPLETA DE LA SUB-TAREA]

Contexto compartido:
[CONTEXTO DEL BRIEF]

Output esperado: [archivo/resultado]. Trabajá solo en tu sub-tarea; no toques nada fuera de su alcance."
```

Modo cruzado o reserva (Composer o Grok):

```bash
bb thread spawn \
  --project "$BB_PROJECT_ID" \
  --parent-self \
  --provider acp-cursor \
  --model <id de Composer o Grok según bb provider models acp-cursor> \
  --reasoning-level medium \
  --prompt "[mismo prompt que arriba]"
```

Capturá los ids (`--json`) y esperá con `bb thread wait <id>` por cada uno. Si una sub-tarea es trivial (renombrar, un grep), hacela vos directo en vez de spawnear — un thread tiene costo fijo de arranque.

---

## Phase 3 — Síntesis (vos, en el thread actual)

Cuando terminen todos:

1. Revisá cada output (`bb thread output <id>`, `bb thread show <id> --git-diff` si tocaron archivos) contra el criterio de éxito del brief.
2. Si algo quedó mal o incompleto, re-spawneá solo esa sub-tarea con el feedback concreto — no repitas las que quedaron bien.
3. Integrá lo que requiera unión (merge de resultados, índice, resumen) vos mismo.
4. Reportá al usuario: qué se hizo, dónde quedó cada output, y qué desviaciones hubo respecto al brief.

---

## Cuándo NO usar este patrón

- Tarea de un solo paso o un solo archivo — hacela directo, la orquestación es puro overhead.
- Sub-tareas fuertemente acopladas entre sí — mejor secuencial en este mismo thread.
- El usuario quiere iterar en vivo sobre el resultado — los threads spawneados no conversan con él.
