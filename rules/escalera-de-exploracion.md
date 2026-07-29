# Escalera de exploración (construir para explorar)

**Regla:** cuando el objetivo es **ambicioso** —una dirección que parece
buena pero cuya forma de largo plazo todavía no está clara—, no se construye
directo. Se propone una **escalera**: visión que filtra, escalones chicos que
componen sobre un mismo cimiento, feedback en cada uno. La escalera se
**consulta y se acuerda antes de construir**.

## Cuándo aplica

Gatillo: el usuario plantea algo grande y de forma incierta — "una versión 2",
una capacidad transversal nueva, "quiero empezar por algo pero no sé cómo se
ve al final".

**No aplica** a trabajo de forma conocida: bugs, ajustes, refactors, una
capacidad ya especificada. Ahí se construye y ya — proponer una escalera es
ceremonia inútil.

## Sub-reglas

1. **Consultar antes de construir.** La escalera se propone como propuesta y
   se acuerda; no se empieza el primer escalón por iniciativa propia aunque
   parezca obvio.
2. **La visión no se implementa: filtra.** Existe para decidir qué cimiento
   no cerrar, no para dictar features. Si empieza a dictar alcance, dejó de
   ser visión y es roadmap encubierto.
3. **Cada escalón entrega valor solo.** Lanzable y útil aunque el resto de la
   escalera nunca se construya. Puede ser rudimentario — se prefiere tosco y
   liberado a acabado y pendiente.
4. **Cada escalón declara su pregunta de feedback.** Qué se quiere aprender al
   liberarlo. Un escalón sin pregunta es infraestructura especulativa
   ([`alcance-disciplinado.md`](alcance-disciplinado.md) §4), no un escalón.
5. **Componer sobre un cimiento.** Declarar explícitamente qué escalón es base
   técnica de cuál, y cuáles son ramas independientes (se pueden adelantar o
   botar sin arrastrar el resto).
6. **Desechable sin arrepentimiento.** Si el feedback mata un escalón, se bota
   sin que caigan los otros. Si botar uno obliga a rehacer todo, la escalera
   estaba mal cortada.
7. **Ordenar por riesgo, no por vistosidad.** Primero el escalón cuyo fracaso
   invalida los siguientes — mejor saberlo en dos semanas que después de
   construir cuatro versiones encima.
8. **La escalera es exploración, no compromiso.** Vive como documento de
   exploración; solo el escalón acordado entra al plan de ejecución.

## Relación con MIW/MIR/MIF

Ejes distintos, no sustitutos. MIW/MIR/MIF es **vertical, dentro de una
capacidad** (funciona → bien hecho → rápido). La escalera es **horizontal,
entre versiones**: qué se construye primero para aprender. Cada escalón se
entrega igual en MIW.

## Por qué

Frente a una idea grande hay dos fracasos simétricos: construir la visión
completa de una (caro, y probablemente equivocado porque nadie sabía todavía
qué era bueno) o quedarse discutiendo hasta que se enfría. La escalera compra
información con entregas chicas: cada una paga su costo con valor real y
devuelve feedback que corrige la siguiente. La composición es lo que evita el
precio habitual de esa táctica — que cada prototipo se bote entero.

Y se consulta antes de construir porque en un objetivo ambiguo el corte de los
escalones **es** la decisión de producto: elegirlo por cuenta propia le quita
al usuario la decisión más importante del trabajo.

## Origen

Formulada por Tomás en conversación (proyecto doterra, 2026-07-28) al planear
una v2 de seguimiento de consultantes. Los cinco puntos originales —visión,
valor, composición, construir, feedback— son suyos; las sub-reglas 4, 6 y 7 y
la distinción con MIW son aportes del agente en esa misma conversación
([`autoria-de-ideas.md`](autoria-de-ideas.md)).
