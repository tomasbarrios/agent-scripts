---
name: css-debug-explain-solve-and-suggest
description: "Diagnosticar y resolver un problema de CSS/layout de forma pedagógica y en 4 fases: navegar y diagnosticar la causa raíz, explicar el concepto desde cero, aplicar la intervención CSS mínima y verificarla, y luego proponer las mejores opciones usando el sistema de estilos del proyecto. Usar cuando el usuario reporta un bug visual o de layout — overflow horizontal, contenido que se sale de la vista, elementos desalineados, scroll inesperado, algo que 'se ve mal' en móvil o desktop — y quiere entender el porqué además de arreglarlo. Trigger en frases como 'se sale del view', 'ocupa más del 100%', 'por qué se ve así', 'overflow', 'no cabe en pantalla', 'debuggear CSS'."
---

# CSS: debug, explica, resuelve y sugiere

Recibe la descripción de un problema de CSS (idealmente: qué vista, qué se ve mal, qué se espera). El resultado no es solo el fix — es que el usuario termine entendiendo la causa, el concepto general, la solución mínima verificada, y las opciones idiomáticas del proyecto. Trabaja las 4 fases en orden y presenta cada una claramente separada.

## Fase 1 — Investiga navegando y diagnostica

No diagnostiques solo leyendo el código fuente: reproduce el problema en un navegador real.

1. Levanta la app (busca cómo en los docs/scripts del repo) y navega a la vista afectada, con el viewport que corresponda (móvil si el reporte es móvil).
2. Inspecciona el DOM computado: encuentra el/los elementos cuyo ancho/alto/posición rompe el layout. Técnica útil para overflow horizontal: recorrer elementos comparando `el.scrollWidth`/`el.getBoundingClientRect()` contra el viewport para encontrar al culpable exacto, o inyectar `* { outline: 1px solid red }` temporalmente.
3. Rastrea la causa en CSS: qué regla, de qué archivo/clase, y por qué produce el comportamiento (ej. `width: 100vw` + scrollbar, padding que se suma por `box-sizing: content-box`, hijo con `white-space: nowrap`, flex item sin `min-width: 0`, imagen sin `max-width`, margen negativo, etc.).

Entrega un **diagnóstico**: elemento culpable, regla culpable, y el mecanismo CSS por el cual esa regla causa lo observado. Cita archivo:línea.

## Fase 2 — Explica el concepto desde cero

Antes de tocar nada, explica conceptualmente cómo se construye bien esta situación si partiéramos de cero — el modelo mental, no el fix puntual. Ejemplo: para un contenido que debe ocupar exactamente el 100%: box model, `box-sizing`, diferencia `100%` vs `100vw`, cómo los contenedores restringen a los hijos, qué elementos tienden a desbordar (tablas, `pre`, imágenes, textos largos sin quiebre) y las defensas idiomáticas contra cada uno.

Mantén la explicación anclada al caso concreto pero generalizable: el usuario debería poder aplicar el concepto la próxima vez sin este skill.

## Fase 3 — Intervención mínima: aplica y verifica

Diseña la intervención más pequeña posible escribiendo CSS adicional (sin refactorizar, sin tocar el sistema de estilos todavía) — primero explícala como ejercicio conceptual: qué regla, dónde, y por qué esa es la mínima.

Luego aplícala y **verifica en el navegador** que el síntoma desapareció y que no rompió otra cosa visible en la misma vista. Reporta el antes/después (screenshot si es posible). Si no funciona, vuelve al diagnóstico — no acumules parches.

Deja claro que este CSS adicional es la solución táctica; la fase 4 evalúa la solución idiomática.

## Fase 4 — Investiga el sistema de estilos del proyecto y propone las mejores opciones

Descubre qué sistema/convención de estilos usa el proyecto: un design system propio (ej. "graffiti"), utilidades tipo Tailwind, CSS modules, tokens, etc. Lee su documentación dentro del repo (busca en `docs/`, `README`, guías de codestyle) y su código antes de opinar.

Aplica el razonamiento de las fases anteriores para responder: ¿cómo se expresa la solución correcta *dentro* de ese sistema? Describe las mejores opciones (normalmente 2–3), cada una con:

- Qué cambiaría y dónde (clases/tokens/componentes del sistema, no CSS suelto).
- Ventajas y costos frente a la intervención mínima de la fase 3.
- Tu recomendación y por qué.

No apliques la opción del sistema sin que el usuario elija — la fase 4 termina en una recomendación, no en más cambios.

## Formato de salida

Estructura la respuesta final con estas secciones: **Diagnóstico**, **Concepto**, **Intervención mínima (aplicada y verificada)**, **Opciones con <sistema del proyecto>** y una recomendación final de una línea.
