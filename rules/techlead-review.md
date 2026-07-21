# Review estilo Tech Lead

**Regla:** al revisar código, ADRs, planes o arquitectura, buscar el cambio
más pequeño que resuelva el problema real y enseñar el criterio detrás.
Aplicar **solo cuando el usuario pida revisar** código, un ADR, un plan o
una decisión de arquitectura — no en cada mensaje.

## Criterios

1. **Simplicidad primero.** ¿Cuál es el cambio más pequeño que resuelve el
   problema real? Comparar la solución contra esa línea base. Si una
   restricción anterior ya no aplica (la operación dejó de ser destructiva,
   el llamador ya tiene el dato recalculado), borrar el código que la
   compensaba en vez de ajustarlo.
2. **Separar presente de futuro.** Distinguir el bug/feature actual de la
   arquitectura futura. No pagar hoy el costo de algo que aún no está
   comprometido en el roadmap.
3. **Cuestionar abstracciones.** Stores globales, singletons, "seams",
   entidades y capas se justifican por una necesidad real y presente, no por
   anticipación.
4. **Contratos explícitos.** Preferir que cada pieza declare qué necesita
   (props/callbacks, firmas claras) sobre dependencias globales implícitas.
5. **Alcance del proyecto.** Respetar el alcance declarado en el `AGENTS.md`
   del repo (MVP, dominio, fases). No modelar reglas de dominio ni capacidades
   fuera de ese alcance salvo petición explícita.
6. **Helpers duplicados → módulo compartido.** Si una función pura (formato
   de fecha, moneda, labels, parsers) aparece igual o casi igual en 2+ módulos,
   sugerir extraerla a un módulo reutilizable con nombre de dominio o
   presentación claro. Severidad: **recomendada** (no bloqueante si el
   duplicado es trivial y local). No inventar un "utils kitchen sink".

## Cómo comunicar

Por cada sugerencia, declarar por separado:

1. **Hallazgo:** qué se observó, con evidencia concreta (archivo/línea).
2. **Criterio aplicado:** cuál de los 6 fundamenta la sugerencia.
3. **Impacto:** qué riesgo, costo o complejidad evita.
4. **Recomendación:** el cambio mínimo y concreto.

- **Severidad:** bloqueante si afecta corrección o datos; recomendada si
  reduce complejidad presente; opcional si solo mejora legibilidad. Lo
  opcional se propone, no entra al diff sin aprobación.
- **Honesto y pedagógico:** explicar el porqué, no solo el veredicto. Si la
  solución no convence, proponer una alternativa concreta (snippet/estructura).
- **Antes / Después / Por qué** cuando el cambio tenga valor de aprendizaje.
- **Una sugerencia por justificación:** no agrupar varias bajo una genérica;
  cada una debe poder aceptarse de forma independiente.
- Si la review revela un criterio reutilizable, proponer guardarlo como regla
  o guía (ver `AGENTS.md` del repo, sección de decisiones).

## Señales de alerta típicas

- "Lo dejo listo para cuando agreguemos X" sin que X esté en el roadmap.
- Estado global introducido para resolver un problema local.
- Un ADR que justifica un cambio pequeño con arquitectura grande.
- Documentación que trata una exploración como decisión tomada.
- La misma función pura copiada en 2+ módulos en vez de un export compartido.

## Por qué

Una review no es solo aprobar/rechazar: protege la simplicidad del proyecto y
deja al autor un criterio reutilizable. El formato Hallazgo/Criterio/Impacto/
Recomendación obliga a fundamentar cada sugerencia, evita listas genéricas y
hace que cada punto se pueda aceptar o rechazar por separado.

## Origen

Extraída de `pasoesencial-cotizador/.cursor/rules/techlead-review.mdc` y
`mvp/docs/devs/review-techlead.md` (2026-07-20), desacoplada del dominio
cotizador/MVP para ser agnóstica de repo.
