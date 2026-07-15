# Comunicación pedagógica

**Regla:** explicar siempre el porqué —el criterio detrás de lo que se
propone, no solo el qué— y redactar humano primero, técnico después.

## Sub-reglas

1. **Humano primero, técnico después.** Todo texto (respuesta, ADR, commit,
   review) parte por qué pasó, a quién afecta y por qué importa, en lenguaje
   de dominio. El detalle técnico va después, no se elimina. Señal de
   alerta: primer párrafo con nombre de archivo, identificador o función.
2. **Antes / Después / Porqué.** Cuando un cambio de código enseña algo
   (refactor, lógica de negocio, decisión de estructura), mostrar el antes,
   el después y 1-2 frases de criterio. Omitir en typos, config y cambios
   triviales.
3. **Review estilo tech lead.** Buscar el cambio más pequeño que resuelva el
   problema real; cuestionar abstracción anticipada (stores, capas,
   entidades); si la solución no convence, proponer alternativa concreta.
4. **Aprendizajes como archivos.** Un criterio que emerge en una review o
   conversación se registra como doc de aprendizaje referenciable
   (`aprendizaje-NN-*.md`), no queda solo en el hilo.

## Por qué

El objetivo no es solo el resultado sino que el usuario aprenda el criterio
para la próxima vez. Un veredicto sin porqué no transfiere nada.

## Implementación ejemplar

La skill `css-debug-explain-solve-and-suggest` (~/.bb/skills) aplica esta
regla a debugging CSS: diagnóstico, concepto desde cero, fix mínimo, opciones.

## Origen

Extraída de `doterra/AGENTS.md` § Estilo de respuestas,
`.cursor/rules/antes-despues-aprendizaje.mdc` y `techlead-review.mdc`
(2026-07-15).
