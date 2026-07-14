# Candidatos a centralización

Índice de reglas, patrones y skills detectados en repos locales que podrían
promoverse a este repo central. Estados: **candidato** → **centralizado**
(o **descartado**). El contenido sigue viviendo en el repo origen hasta que
se promueve — este archivo es solo el índice.

- **Fuentes escaneadas (2026-07-14):** `~/own/business/doterra`, `~/.bb/skills/`
- **Evaluación (2026-07-14):** notas 1-10 asignadas por Tomás en la tabla; el
  detalle de cada cluster incluye una sección **Observación mía** con su
  comentario. Pendiente: revisar el detalle por cluster.
- **Pregunta abierta:** al centralizar una regla, ¿qué pasa en el repo origen?
  (puntero al central / copia divergente / no tocar). Sin resolver — ver
  [`docs/design.md`](docs/design.md) § patrones de distribución.

## Resumen de clusters

Notas 1-10 asignadas por Tomás (2026-07-14). Las observaciones extendidas van en el detalle de cada cluster, como blockquote **Observación mía**, separadas del punteo descriptivo.

| # | Cluster | Subclusters | Evidencia principal | Cobertura central | Nota (tú) |
|---|---------|-------------|--------------------|-------------------|-----------|
| 1 | **Etapas de desarrollo (MIW/MIR/MIF)** | definición de fases; qué NO es cada fase; cierre de fase con backlog | `doterra/mvp/docs/devs/process-miw.md`, ADR-003, `AGENTS.md` § Alcance | Ninguna | **10** |
| 2 | **Specs/docs antes de programar** | taxonomía documental (analysis/design/exploration/ADR/devs/domain/product); flujo INBOX → hipótesis → opportunities → ROADMAP; "Narrativa ≠ spec" | `doterra/mvp/docs/README.md`, `inbox-taxonomia.md`, `.cursor/commands/process-inbox.md` | Ninguna | **10** |
| 3 | **Navegación por README / eficiencia de contexto** | README como enrutador (no resumen); regla de navegación en 4 pasos; detalle proporcional a la jerarquía; mantenimiento del README como índice fiel | `doterra/AGENTS.md` § Navegación por README | Ninguna | **8** — es mi método actual, pero no necesariamente quiero mantenerlo |
| 4 | **Declaración vs explicación (regla corta → doc extendida)** | patrón `.cursor/rules/*.mdc` con "Fuente de verdad: docs/..."; filosofía "si es valioso para un humano, no va en archivo oculto" | las 5 reglas de `doterra/.cursor/rules/` siguen este patrón | Ninguna (es TU meta-patrón — buen primer centralizado) | **8** — me gusta centralizar la explicación |
| 5 | **Alcance disciplinado / YAGNI** | solo lo aprobado (opcional/MIR/considerar no se codifica); sin comportamiento DOM sorpresa; no infra especulativa sin dolor repetido; no salir del repo sin aprobación | `doterra/AGENTS.md` § YAGNI y § Alcance, `.cursor/rules/alcance-implementacion.mdc` | Ninguna | **10** |
| 6 | **Comunicación pedagógica** | humano primero / técnico después; explicar el porqué; Antes/Después/Porqué en cambios de código; review estilo tech lead; archivos `aprendizaje-NN` | `doterra/AGENTS.md` § Estilo, `.cursor/rules/antes-despues-aprendizaje.mdc`, `techlead-review.mdc`, `devs/aprendizaje-0*.md` | Ninguna | **10** |
| 7 | **Orquestación y economía de modelos** | tabla tarea→tipo de agente→modelo; delegar mecánico a modelos gratis; skills orchestrate / orchestrate-opencode | `doterra/AGENTS.md` § Enrutamiento, `~/.bb/skills/orchestrate*`, `doterra/.bb/skills/orchestrate-opencode` (¡duplicada en 2 fuentes!) | Parcial (skills viven en ~/.bb, no acá) | **9** |
| 8 | **Documentación continua ("no repetir lo mismo en otro contexto")** | cada aclaración → evaluar si documentarla; reglas de agente van a AGENTS.md, nunca solo a memoria privada; hooks/skills creados deben documentarse en el README más cercano | `doterra/AGENTS.md` § Decisiones y preguntas, § Programación | Ninguna | **10** |
| 9 | **Captura y release bajo demanda** | INBOX.md + taxonomía + /process-inbox; CHANGELOG solo al tag; /sync-docs contrastando main; /tag-release | `doterra/INBOX.md`, `.cursor/commands/{process-inbox,sync-docs,tag-release}.md` | Ninguna | **5** — proceso débil actualmente |
| 10 | **Skills de diagnóstico y producto** | css-debug pedagógico en 4 fases; product-expert-definition-of-ready; design-look-feel; screenshot | `~/.bb/skills/` (4 skills) | Parcial (viven en ~/.bb, no en este repo) | **Sin nota** — agruparlas es un error; desagrupar según el tema de cada skill |
| 11 | **Codestyles con fuente de verdad** | Graffiti class-first + tokens; estado Svelte local-first; vocabulario de dominio en nombres | `doterra/.cursor/rules/{graffiti,svelte-state}-codestyle.mdc`, `devs/codestyles/` | Ninguna — probablemente NO centralizable (específico de doterra), salvo el meta-patrón | **Sin nota** — muy particular al repo y sus herramientas, difícil de portar; buscar reglas redactadas de forma agnóstica |
| 12 | **Herramientas y fuentes acotadas** | allowlist de rutas del vault (`context/sources.md`); scope estricto de browser-gateway | `doterra/AGENTS.md` § Fuentes, § Programación | Ninguna | **4** |
| 13 | **Narrativa de producto como artefacto** *(propuesto)* | narrativa antes que spec; narrativas UX como documento propio; evaluación por dimensiones de persuasión (narrativa, framing, copy, conducta); narrativa como criterio de ready | "Narrativa ≠ spec" (extraído del cluster 2), `~/.bb/skills/product-expert-definition-of-ready`, `design-look-feel` (brief como narrativa visual) | Parcial (la skill vive en ~/.bb) | pendiente de evaluar |

## Detalle por cluster

### 1. Etapas de desarrollo (MIW/MIR/MIF)
- Fases secuenciales por capacidad, no niveles de alcance: MIW (¿funciona?) → MIR (¿bien hecho?) → MIF (¿rápido?). Fuente: `mvp/docs/devs/process-miw.md`.
- Cada fase tiene un "qué NO es" explícito (MIR no amplía alcance; MIF no agrega capacidades).
- Al cerrar MIW, lo opcional que surgió va a backlog en el resumen, nunca mergeado en silencio.
- **Generalizable:** sí — el marco es agnóstico al proyecto; solo los ejemplos son de doterra.

> **Observación mía (nota 10):** —

### 2. Specs/docs antes de programar
- Taxonomía de carpetas con regla de clasificación rápida por tipo de pregunta ("¿qué decidimos?" → ADR; "¿cómo lo implementamos?" → devs/; "¿qué exploramos?" → design/). Fuente: `mvp/docs/README.md`.
- Pipeline de decantación: INBOX → `exploration/hipotesis.md` (sin validar) → `product/opportunities.md` (validado) → ROADMAP (comprometido). El ROADMAP solo contiene trabajo decidido.
- "Narrativa ≠ spec": exploración UX va a docs, no a código, hasta pedido explícito.
- **Generalizable:** la taxonomía y el pipeline sí; los nombres de carpeta podrían variar por repo.

> **Observación mía (nota 10):** — (ver también cluster 13: el tema "Narrativa ≠ spec" se extrae como dimensión propia)

### 3. Navegación por README / eficiencia de contexto
- README = enrutador por tipo de pregunta, no resumen de los hijos; detalle proporcional a la posición en la jerarquía.
- Regla de navegación para agentes: leer README antes de abrir archivos; sin pista en ningún README → responder "no encontrado", no explorar a ciegas.
- Cada README debe mantenerse como índice fiel; README desactualizado → proponer actualizarlo.
- **Generalizable:** completamente — es de las reglas más maduras y agnósticas que tienes.

> **Observación mía (nota 8):** es mi método actual, pero no es que quiera mantenerlo necesariamente.

### 4. Declaración vs explicación (tu meta-patrón)
- La regla vive corta en el archivo que la herramienta lee (`.mdc`, `AGENTS.md`); la explicación vive en un doc visible para humanos, y la regla apunta ahí ("Fuente de verdad: ...").
- Filosofía: "si es valioso para un humano no debería estar en un archivo oculto".
- Pendiente declarado por ti: investigar eficiencia de tokens de este patrón; futura herramienta para auditar qué tipo de contenido va en cada tipo de archivo.
- **Generalizable:** es el candidato natural a PRIMERA regla centralizada — todas las demás se escriben siguiendo este patrón.

> **Observación mía (nota 8):** me gusta centralizar la explicación.

### 5. Alcance disciplinado / YAGNI
- Solo ítems obligatorios del plan o pedido explícito; *opcional / MIR / si trivial / considerar* → proponer, no implementar.
- Sin DOM sorpresa (scroll automático, toasts, animaciones, undo) sin pedido explícito.
- No infraestructura especulativa sin dolor concreto repetido; preferir un archivo + un script.
- Trabajo fuera del repo actual requiere aprobación explícita.
- **Nota:** § Alcance está duplicado dos veces en `AGENTS.md` (líneas 48-61) — señal de que falta la fuente única que este repo daría.

> **Observación mía (nota 10):** —

### 6. Comunicación pedagógica
- Humano primero, técnico después: primer párrafo en lenguaje de dominio; señal de alerta si arranca con nombre de archivo/función.
- Explicar siempre el porqué; formato Antes/Después/Porqué cuando el cambio enseña algo (con criterios de cuándo aplicar y cuándo omitir).
- Review tech lead: buscar el cambio más pequeño, cuestionar abstracción anticipada, comunicar honesto y pedagógico.
- Los aprendizajes se registran como archivos `aprendizaje-NN-*.md` referenciables desde reglas.
- **Generalizable:** sí, casi textual.

> **Observación mía (nota 10):** —

### 7. Orquestación y economía de modelos
- Clasificar tarea → tipo de agente → modelo (mecánico → opencode free; código → Sonnet; diseño/arquitectura → directo sin delegar).
- **Duplicación detectada:** `orchestrate-opencode` existe en `~/.bb/skills/` Y en `doterra/.bb/skills/` — primer caso concreto de la pregunta abierta 4 (¿cuál es canónica?).

> **Observación mía (nota 9):** —

### 8. Documentación continua
- Cada aclaración en conversación → evaluar si es candidata a documentarse y proponerlo ("no queremos repetir lo mismo en otro contexto").
- Reglas de agente van a archivos del repo, nunca solo a memoria privada del agente.
- Todo hook/skill creado queda documentado en el README más cercano: qué dispara, qué hace, por qué existe.
- **Nota:** esta regla es la semilla conceptual de ESTE MISMO repo — el flujo candidato→centralizado es su versión inter-repos.

> **Observación mía (nota 10):** —

### 9. Captura y release bajo demanda
- INBOX.md con tags de taxonomía; procesar con plan por ítem → aprobación → destino → archivado.
- CHANGELOG solo se escribe al cerrar release; docs de producto se sincronizan bajo demanda, no en cascada.
- **Generalizable:** el patrón INBOX sí; los comandos son portables con ajustes de rutas.

> **Observación mía (nota 5):** el proceso es débil actualmente.

### 10. Skills de diagnóstico y producto (~/.bb/skills)
- `css-debug-explain-solve-and-suggest` — diagnóstico CSS pedagógico en 4 fases.
- `product-expert-definition-of-ready` — evaluación de cambios de producto en 4 dimensiones.
- `design-look-feel` — proceso de diseño con control de costo por modelo.
- `screenshot` — capturas de HTML con Playwright.
- **Ya son "centralizadas" de facto** (globales en ~/.bb) — la decisión es si este repo pasa a ser su fuente de verdad versionada.

> **Observación mía (sin nota):** agrupar esto es un error — desagruparlas según el tema de cada una. Propuesta de reasignación: `css-debug` → cluster 6 (comunicación pedagógica); `product-expert-definition-of-ready` → cluster 13 (narrativas y persuasión); `design-look-feel` → cluster 7 (economía de modelos) y cluster 13; `screenshot` → utilidad suelta (herramienta, cluster 12 o ninguno).

### 11. Codestyles con fuente de verdad (probablemente NO centralizar)
- Graffiti class-first, estado Svelte local-first, vocabulario de dominio: específicos de doterra.
- Lo centralizable es el **meta-patrón**: cada codestyle = ADR (decisión) + guía operativa (devs/) + regla corta (.mdc) que apunta a ambas.

> **Observación mía (sin nota):** muy particular al repositorio y las herramientas, difícil de portar. Habría que buscar reglas redactadas de forma agnóstica.

### 12. Herramientas y fuentes acotadas
- Allowlist explícita de rutas externas (`context/sources.md`); no explorar el resto del vault.
- Tools MCP con scope declarado (browser-gateway solo para sus tools acotadas).
- **Generalizable:** el patrón allowlist sí; los tools concretos dependen de cada máquina.

> **Observación mía (nota 4):** —

### 13. Narrativa de producto como artefacto (propuesto — dimensión nueva)
Dimensión propia para el tema de las narrativas, hoy disperso en otros clusters:
- **Narrativa antes que spec** (extraído del cluster 2): la exploración UX se escribe como narrativa en docs y no se convierte en código hasta pedido explícito. La narrativa es un artefacto de primera clase, con su propio lugar en la taxonomía documental.
- **Narrativa como lente de evaluación**: `product-expert-definition-of-ready` evalúa cambios de producto por dimensiones de comunicación y persuasión (narrativa, diseño conductual, copy, framing) ponderadas por momento del user journey.
- **Narrativa visual**: `design-look-feel` produce un brief de diseño que es, en el fondo, una narrativa del look & feel antes de implementar.
- **Posible regla centralizable**: "toda feature nace como narrativa (qué historia le cuenta al usuario) antes de tener spec técnica; la narrativa se evalúa por sí misma antes de comprometer trabajo".

> **Observación mía:** pendiente de evaluar — cluster creado a partir de mi pedido de una dimensión propia para narrativas y temas relacionados.
