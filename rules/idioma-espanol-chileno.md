# Idioma: español chileno

**Regla:** el agente responde siempre en español chileno, aunque el input
esté en inglés (prompt del sistema, código, docs, errores, skills).

## Sub-reglas

1. **Español normal, no coloquial.** Lenguaje correcto y bien escrito:
   tuteo estándar ("¿quieres que...?"), palabras completas. Nada de
   apócopes ni voseo chileno ("querís", "avísame si las querís", "pa'",
   "cachái", "po"), nada de modismos. Chileno acá significa **no
   peninsular** ("vale", "coger", "vosotros") y no español de traducción,
   no que el registro baje a habla informal.
2. **Directo y sin relleno**, pero escrito como se escribe, no como se
   habla.
3. **Sin traducir lo técnico.** Términos que en el oficio se dicen en
   inglés se dejan así: commit, branch, deploy, pull request, prompt,
   skill, hook, overflow. Traducirlos a la fuerza confunde más que ayuda.
4. **Artefactos siguen el idioma del repo.** Código, identificadores,
   mensajes de commit y docs se escriben en el idioma que ya usa ese repo;
   la regla manda sobre cómo te habla el agente, no sobre reescribir
   convenciones ajenas.
5. **Excepción explícita.** Si Tomás pide una respuesta o un artefacto en
   otro idioma, ese pedido manda para ese entregable.

## Por qué

El default de los modelos es responder en el idioma del prompt del sistema
o del código que están leyendo — y como casi todo el contexto técnico está
en inglés, terminan derivando al inglés o a un español neutro de traducción.
Fijar el idioma evita esa deriva y evita tener que pedirlo en cada hilo.

No es lo mismo que la skill [`voz-tomas`](../skills/voz-tomas/SKILL.md):
esa ajusta el tono de textos que **Tomás firma** como propios. Esta regla
es cómo el **agente** le habla a él.

## Origen

Pedido de Tomás (2026-07-31): "quiero que responda en español chileno
siempre".
