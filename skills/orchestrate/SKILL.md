---
name: orchestrate
description: "Orquestar cualquier tarea grande dividiéndola en plan + ejecución delegada: Fable 5 piensa, diseña el plan y sintetiza; agentes baratos (Sonnet 5) ejecutan las partes en paralelo. Sirve para cualquier dominio — código, investigación, documentos, migraciones, contenido. Usar cuando el usuario pide 'orquestar', 'divide y ejecuta', 'usa agentes baratos', 'planifica con Fable y delega', o cuando una tarea tiene varias partes independientes que conviene paralelizar con costo mínimo."
---

# Orchestrate — Plan caro, ejecución barata

Patrón general de tres fases, agnóstico al dominio. La idea: el juicio y la descomposición valen tokens caros una sola vez; la ejecución mecánica no.

| Fase | Modelo | Por qué |
|------|--------|---------|
| Planning + Work Brief | Fable 5 (thread actual) | La descomposición correcta es lo que más valor agrega |
| Ejecución de tareas | Sonnet 5 (threads paralelos) | Trabajo mecánico spec-a-resultado — Fable no agrega valor acá |
| Síntesis / revisión | Fable 5 (thread actual) | Juntar resultados y juzgar calidad requiere criterio |

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

## Phase 2 — Ejecución (Sonnet 5, paralelo)

Spawná **un thread por sub-tarea**, todos en paralelo:

```bash
bb thread spawn \
  --project "$BB_PROJECT_ID" \
  --parent-self \
  --model claude-sonnet-5 \
  --prompt "[INSTRUCCIÓN COMPLETA DE LA SUB-TAREA]

Contexto compartido:
[CONTEXTO DEL BRIEF]

Output esperado: [archivo/resultado]. Trabajá solo en tu sub-tarea; no toques nada fuera de su alcance."
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
