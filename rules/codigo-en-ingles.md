# Código en inglés

**Regla:** todo artefacto de código —identificadores, mensajes de commit,
nombres de archivos, strings de UI/log salvo que el dominio exija otro
idioma— se escribe en inglés, sin importar el idioma en que se converse
con el agente. **Los comentarios son la excepción: pueden ir en español
chileno.**

## Sub-reglas

1. **Aplica a lo que se escribe, no a lo que se lee.** Si un repo ya tiene
   convenciones en otro idioma (código legado, dominio en español), no se
   reescribe solo por esta regla; se sigue la convención existente del
   archivo o módulo que se está tocando. Esta regla rige código **nuevo**
   y decide el idioma en casos ambiguos.
2. **Comentarios en español chileno.** A diferencia del resto del código,
   los comentarios pueden escribirse en español chileno (misma variante de
   [`idioma-espanol-chileno.md`](idioma-espanol-chileno.md): sin modismos
   ni voseo, términos técnicos en inglés). Si un archivo ya tiene
   comentarios en inglés, se sigue esa convención existente en vez de
   mezclar idiomas dentro del mismo archivo.
3. **No compite con el idioma de conversación.** Que el agente responda en
   español chileno no cambia el idioma de los identificadores, commits o
   docs que ese mismo turno produce.
4. **Excepción explícita.** Si Tomás pide código en otro idioma para un
   entregable puntual, ese pedido manda para ese entregable.
5. **Textos de cara al usuario final** (copy de producto, contenido) no
   son "código" para efectos de esta regla — siguen el idioma que el
   producto ya usa con sus usuarios, no esta regla.

## Por qué

El código en inglés es el estándar de facto de la industria: facilita
lectura a colaboradores externos, matchea con librerías/APIs/docs que ya
están en inglés, y evita mezclar idiomas dentro de un mismo archivo cuando
el agente conversa en español pero escribe identificadores.

## Origen

Pedido de Tomás (2026-08-18): impone la regla tras preguntar si existía.
