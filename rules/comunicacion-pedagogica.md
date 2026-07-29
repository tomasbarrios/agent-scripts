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
3. **Comentarios de causa no obvia.** Cuando el código se ve más simple que
   su razón —alguien sin la historia lo leería como redundante o borrable—
   el comentario va en tres capas: **síntoma** observable en lenguaje humano
   (sin nombres de clases ni funciones), **¿por qué?** con el mecanismo
   técnico, y **solución actual** con qué hace esto y por qué esta y no
   otra. Si la solución es táctica, decirlo: marca el punto como refactor
   pendiente y localizable.

   ```
   /* Fix: <síntoma observable, sin jerga>
      ¿Por qué? <qué regla o comportamiento lo causa>
      Solución actual: <qué hace esto, y por qué esta y no otra> */
   ```

   Disparadores típicos: regla CSS que parece duplicar al framework, `if`
   aparentemente inalcanzable, `await`/timeout que "no hace nada", versión
   pineada, orden de sentencias que importa sin que se note, `catch` que
   traga un error a propósito.
4. **Review estilo tech lead.** Buscar el cambio más pequeño que resuelva el
   problema real; cuestionar abstracción anticipada (stores, capas,
   entidades); si la solución no convence, proponer alternativa concreta.
5. **Aprendizajes como archivos.** Un criterio que emerge en una review o
   conversación se registra como doc de aprendizaje referenciable
   (`aprendizaje-NN-*.md`), no queda solo en el hilo.

## Por qué

El objetivo no es solo el resultado sino que el usuario aprenda el criterio
para la próxima vez. Un veredicto sin porqué no transfiere nada.

## Implementación ejemplar

La skill [`css-debug-explain-solve-and-suggest`](../skills/css-debug-explain-solve-and-suggest/)
aplica esta regla a debugging CSS: diagnóstico, concepto desde cero, fix mínimo, opciones.

## Origen

Extraída de `doterra/AGENTS.md` § Estilo de respuestas,
`.cursor/rules/antes-despues-aprendizaje.mdc` y `techlead-review.mdc`
(2026-07-15). Sub-regla 3 (comentarios de causa no obvia) agregada a partir
de un fix CSS de `.stack` vs `dialog` en Graffiti (2026-07-28).
