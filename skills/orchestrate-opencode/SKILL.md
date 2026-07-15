---
name: orchestrate-opencode
description: "Orquestar una tarea grande con planificación en Sonnet 5 y ejecución de sub-tareas triviales/mecánicas en modelos gratuitos de OpenCode (provider acp-opencode, ej. opencode/big-pickle, opencode/north-mini-code-free). Variante de bajo costo de 'orchestrate': usar cuando el usuario pide 'orquestar con opencode', 'usa opencode para las tareas triviales', 'ejecuta con modelos gratis', o cuando el trabajo se descompone en muchas sub-tareas mecánicas (renombrar, extraer texto, formatear, traducir líneas, generar boilerplate repetitivo) donde no hace falta buen juicio de código. Si las sub-tareas requieren razonamiento, diseño, o decisiones de arquitectura, usar el skill 'orchestrate' (Sonnet) en vez de este."
---

# Orchestrate OpenCode — Plan en Sonnet, ejecución mecánica gratis

Variante de `orchestrate` para cuando la mayoría de las sub-tareas son mecánicas: no requieren criterio, solo ejecutar una instrucción bien especificada. En vez de gastar Sonnet en eso, se delega a modelos gratuitos de OpenCode ("OpenCode Zen") vía provider `acp-opencode`.

| Fase | Modelo | Por qué |
|------|--------|---------|
| Planning + Work Brief | Sonnet 5 (thread actual) | La descomposición correcta y evaluar qué es "trivial" requiere criterio |
| Ejecución de sub-tareas mecánicas | OpenCode Zen (`acp-opencode`, threads paralelos) | Trabajo spec-a-resultado sin ambigüedad — no hace falta un modelo caro, y estos modelos son gratis |
| Síntesis / revisión | Sonnet 5 (thread actual) | Juntar resultados y detectar errores de los modelos baratos requiere criterio |

**Contrapartida:** los modelos de OpenCode Zen son gratuitos pero más débiles que Sonnet. Sirven para tareas mecánicas bien acotadas; para sub-tareas que involucren lógica de negocio, arquitectura, o código no trivial, esa sub-tarea puntual conviene mandarla a Sonnet (`orchestrate` normal) aunque el resto del lote vaya a OpenCode.

---

## Phase 1 — Planning (quedate en el thread actual, Sonnet 5)

Trabajá iterativamente con el usuario hasta entender el objetivo. El entregable es un **Work Brief** igual que en `orchestrate`, pero con un campo extra: para cada sub-tarea, decidí si es **mecánica** (va a OpenCode) o **requiere criterio** (va a Sonnet). Ante la duda, mandala a Sonnet — el ahorro de una sub-tarea no vale un resultado mal hecho que hay que rehacer.

Sub-tareas mecánicas típicas: extraer/formatear datos, renombrar en lote, traducir texto plano, generar variaciones de un template, aplicar el mismo cambio repetitivo en muchos archivos con la misma forma.
Sub-tareas que NO son mecánicas: decisiones de diseño, código con lógica de negocio, cualquier cosa donde "depende" sea una respuesta válida.

### Work Brief format

```
## Work Brief

**Objetivo:** [una frase]
**Contexto compartido:** [lo que TODOS los agentes necesitan saber: rutas, convenciones, restricciones]

### Sub-tareas
1. [nombre] — [instrucción completa] — Ejecutor: OpenCode | Sonnet — Output: [archivo/resultado esperado]
2. ...

### Criterio de éxito
- [cómo sabemos que el conjunto quedó bien]
```

Iterá hasta aprobación explícita del usuario ("listo", "dale", "procede", "ok sigamos"). No spawnees nada antes de eso.

---

## Phase 2 — Ejecución (paralelo, modelo según ejecutor)

Antes de spawnear, confirmá qué modelos de OpenCode Zen hay disponibles ahora mismo (la lista puede cambiar):

```bash
bb provider models acp-opencode
```

Spawná **un thread por sub-tarea**, todos en paralelo. Para sub-tareas mecánicas:

```bash
bb thread spawn \
  --project "$BB_PROJECT_ID" \
  --parent-self \
  --provider acp-opencode \
  --model opencode/big-pickle \
  --prompt "[INSTRUCCIÓN COMPLETA DE LA SUB-TAREA]

Contexto compartido:
[CONTEXTO DEL BRIEF]

Output esperado: [archivo/resultado]. Trabajá solo en tu sub-tarea; no toques nada fuera de su alcance."
```

Para sub-tareas de código específicamente, `opencode/north-mini-code-free` suele rendir mejor que `big-pickle`. Para las sub-tareas marcadas "Ejecutor: Sonnet" en el brief, spawneá igual que en `orchestrate` (`--model claude-sonnet-5`, sin `--provider`).

Capturá los ids (`--json`) y esperá con `bb thread wait <id>` por cada uno. Si una sub-tarea es tan chica que ni vale un thread (renombrar una cosa, un grep), hacela vos directo — el spawn tiene costo fijo de arranque.

---

## Phase 3 — Síntesis (vos, Sonnet 5, en el thread actual)

Cuando terminen todos:

1. Revisá cada output (`bb thread output <id>`, `bb thread show <id> --git-diff` si tocaron archivos) contra el criterio de éxito del brief. Prestá más atención a los outputs de OpenCode — son el eslabón más débil de la cadena.
2. Si algo quedó mal, primero evaluá si fue por ambigüedad del modelo barato o porque la sub-tarea era menos "mecánica" de lo que parecía. En el segundo caso, re-spawneala en Sonnet en vez de reintentar en OpenCode.
3. Integrá lo que requiera unión (merge de resultados, índice, resumen) vos mismo.
4. Reportá al usuario: qué se hizo, qué ejecutor usó cada sub-tarea, dónde quedó cada output, y qué desviaciones hubo respecto al brief.

---

## Cuándo NO usar este patrón

- Tarea de un solo paso o un solo archivo — hacela directo.
- La mayoría de las sub-tareas requieren criterio de código o de producto — usá `orchestrate` (todo en Sonnet) en vez de este.
- Sub-tareas fuertemente acopladas entre sí — mejor secuencial en este mismo thread.
- El usuario quiere iterar en vivo sobre el resultado — los threads spawneados no conversan con él.
