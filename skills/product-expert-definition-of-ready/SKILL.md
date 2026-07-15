---
name: product-expert-definition-of-ready
description: 'Evalúa un cambio propuesto en un producto digital (feature, copy, flujo, pantalla) usando 4 dimensiones de comunicación y persuasión — narrativa, diseño conductual, copy persuasivo, framing — ponderadas según el momento del user journey donde ocurre el cambio. Usar cuando el usuario pregunta si una feature/copy/flujo es buena idea, pide evaluar o priorizar un cambio de producto, duda si lanzar algo "no perfecto", menciona fricción de onboarding, registration wall, churn, activación, retención, o pide ayuda para decidir entre varias propuestas de producto. También usar para generar el reporte de evaluación en formato tabla y, si el resultado se va a compartir con alguien que no conoce el sistema, para producir la guía explicativa adjunta.'
---

# Evaluador de cambios de producto

Método para decidir si un cambio propuesto en un producto digital vale la pena lanzarlo, identificando dónde hay riesgo real en vez de exigir que sea perfecto en todo. Nació de la idea de que las decisiones humanas son mayormente emocionales, y que un cambio de producto comunica (explícita o implícitamente) de 4 formas distintas — cada una relevante en un momento distinto del recorrido del usuario.

## Las 4 dimensiones

1. **Narrativa** — la historia que la persona se cuenta a sí misma sobre por qué usa el producto; identidad y sentido a través del tiempo, no un mensaje puntual. (Campbell, Booker, McKee, Gottschall — _The Storytelling Animal_, Donald Miller — _StoryBrand_)
2. **Diseño conductual** — qué tan fácil es actuar YA, en el instante, sin importar cuán convencida esté la persona. Modelo de Fogg: Motivación × Habilidad × Trigger. Modelo de Hooked (Nir Eyal): Trigger → Acción → Recompensa variable → Inversión.
3. **Copy persuasivo** — el texto puntual que resuelve una objeción o activa una decisión en el momento exacto de duda. (Cialdini — _Influence_, Schwartz — _Breakthrough Advertising_, Ogilvy)
4. **Framing** — cómo se enmarca la misma información para que se lea distinto; el mismo dato, dicho de otra forma, cambia la decisión. (Lakoff — _Don't Think of an Elephant_, Kahneman/Tversky — prospect theory)

Narrativa y diseño conductual pesan más en los extremos del journey (por qué + hábito). Copy y framing pesan más en los momentos de decisión explícita (entrada y salida/riesgo).

## Relevancia por defecto según el momento del journey

Usar esta tabla como punto de partida; ajustarla si el producto real se comporta distinto.

| Momento                                          | Narrativa | Conductual | Copy  | Framing |
| ------------------------------------------------ | --------- | ---------- | ----- | ------- |
| 1. Descubrimiento (primer contacto, landing, ad) | Media     | Baja       | Alta  | Alta    |
| 2. Onboarding (primer uso)                       | Baja      | Alta       | Media | Media   |
| 3. Activación (aha moment)                       | Alta      | Alta       | Baja  | Baja    |
| 4. Uso habitual / retención                      | Alta      | Alta       | Baja  | Baja    |
| 5. Fricción / riesgo de abandono (churn)         | Media     | Media      | Alta  | Alta    |
| 6. Reactivación / upsell / renovación            | Alta      | Media      | Alta  | Media   |

## Cómo evaluar un cambio

1. **Identificar el/los momentos del journey que toca el cambio.** Un cambio puede tocar más de uno (ej. quitar una barrera en Descubrimiento y agregar un paso nuevo en Activación) — evaluar cada momento por separado, no mezclarlos en una sola tabla.
2. **Por cada momento, llenar esta tabla** (ver `assets/plantilla.md` para la versión en blanco):

   | Dimensión | Relevancia (1-5) | Nota (1-5) | Riesgo |
   | --------- | ---------------- | ---------- | ------ |
   - **Relevancia**: cuánto importa esa dimensión en ESE momento, independiente de la idea propuesta. Sale de la tabla por defecto, ajustada al producto si hace falta.
   - **Nota**: qué tan bien la idea propuesta resuelve esa dimensión. Esto sí requiere juicio sobre la propuesta concreta.
   - **Riesgo = Relevancia − Nota.**

3. **Ordenar siempre las filas por Riesgo descendente.** Lo primero que se lee es dónde hay riesgo real (relevancia alta + nota baja); lo que tiene riesgo bajo o negativo va al final porque no requiere atención inmediata.
4. **Aplicar el sistema de color** (ver abajo) a cada fila.
5. **Leer el resultado**: el objetivo no es que el cambio saque nota perfecta en las 4 dimensiones — es identificar las filas rojas (relevancia alta, mal resuelto) y decidir si se corrigen antes de lanzar o se aceptan como riesgo consciente.

## Sistema de color (4 estados)

"Riesgo bajo" puede pasar por dos motivos muy distintos, y mezclarlos en un solo color pierde información: puede ser porque la dimensión está **bien resuelta** (nota alta) o porque **no importaba ahí** (relevancia baja). Por eso son 4 estados, no 3:

| Color       | Condición                                  | Significado                                                                                                |
| ----------- | ------------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| 🔴 Rojo     | Riesgo alto (relevancia alta, nota baja)   | Corregir antes de lanzar                                                                                   |
| 🟡 Amarillo | Riesgo medio                               | Vigilar, no bloquea el lanzamiento                                                                         |
| 🟢 Verde    | Riesgo bajo/negativo **y** relevancia alta | Fortaleza real — está bien resuelto donde importaba                                                        |
| ⚪ Gris     | Riesgo bajo/negativo **y** relevancia baja | No aplica en este momento — no dice nada sobre la calidad de la idea, simplemente no había que mirarlo ahí |

Verde y gris se ven igual en el número de Riesgo pero significan cosas opuestas: verde es una fortaleza a preservar, gris es indiferencia. No los trates como lo mismo al leer o comunicar el resultado.

## Umbral de decisión: todavía no lo fijes

No existe (todavía) una regla dura de "score compuesto mínimo para aprobar". La recomendación es no inventar un umbral rígido de entrada — usar el método en 3-4 cambios reales, ver qué filas rojas terminaron importando de verdad en los resultados, y recién ahí calibrar un corte con evidencia propia.

Como punto de partida tentativo (no como regla fija): una fila con **Riesgo ≥ 2 y Relevancia ≥ 4** amerita revisión antes de lanzar. Todo lo demás es zona gris a criterio de quien decide.

## Ejemplo trabajado

**Cambio**: reemplazar un _registration wall_ (pedir email obligatorio para VER una wishlist) por pedir el email solo cuando el usuario quiere **asignar/adjudicar** un regalo de la lista.

Este cambio toca dos momentos: Descubrimiento (ver la lista, ahora sin barrera) y una especie de mini-Activación en el instante de asignar (se pide el dato en el momento de mayor intención — patrón Hooked de pedir "inversión" recién después de mostrar valor).

**Momento 2 — Asignación** (ordenado por Riesgo):

| Dimensión  | Relevancia | Nota | Riesgo | Color |
| ---------- | ---------- | ---- | ------ | ----- |
| Copy       | 4          | 2    | 2      | 🔴    |
| Conductual | 5          | 4    | 1      | 🟡    |
| Framing    | 3          | 2    | 1      | 🟡    |
| Narrativa  | 2          | 3    | -1     | ⚪    |

Lectura: el timing conductual está bien resuelto (pide el dato justo cuando hay intención real). El riesgo real está en el copy del modal — aún no explica _para qué_ se pide el email ("para avisarte si alguien más ya lo reservó"). Eso es lo único que vale la pena resolver antes de lanzar; el resto no bloquea.

## Compartir el resultado con alguien que no conoce el sistema

Si vas a mandar esta evaluación a alguien (diseño, dev, otro founder) que no conoce las 4 dimensiones ni el sistema de Riesgo, no asumas que la tabla se explica sola. Usa `references/guia-explicativa.md` — tiene una explicación simple de cada dimensión con un ejemplo inventado (mala idea vs. buena idea) para cada una, agnóstico a cualquier producto real.
