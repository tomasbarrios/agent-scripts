---
name: scratch-review
description: Revisa la carpeta .scratch/ (gitignoreada) de un repo item por item, decidiendo con el usuario si cada cosa se promueve a un lugar versionado (docs/, CONTEXT.md, un ADR) o se purga porque ya cumplió su función. Usar cuando el usuario pide limpiar, revisar, purgar u ordenar su scratch/mesa de trabajo, cuando .scratch/ acumula research, tickets o screenshots viejos, o cuando el usuario duda si algo generado ahí vale la pena guardar en el repo de verdad.
---

# Scratch review

`.scratch/` es mesa de trabajo local: research, tickets, screenshots y notas
efímeras que un agente deja mientras investiga, gitignoreadas a propósito
para no ensuciar el historial con material a medio cocinar. Con el tiempo
se acumulan ahí dos cosas mezcladas: hallazgos que ya deberían vivir en un
doc versionado, y basura que ya cumplió su función (un screenshot puntual
de debug, un dump temporal). Nadie las separa solo porque nadie las mira.

Esta skill hace esa separación, item por item, con el usuario decidiendo
qué es valioso — esa decisión no es del agente.

## Antes de empezar

1. Confirmar que existe `.scratch/` en la raíz del repo (o donde el usuario
   indique). Si no existe, decirlo y parar — no es una carpeta a crear.
2. Confirmar que está gitignoreada (`git check-ignore .scratch`). Si no lo
   está, preguntar si igual se quiere hacer la revisión — el flujo de
   promover/purgar aplica igual, pero el motivo de "purgar sin culpa" es
   más débil si ya está versionado.
3. Con `git ls-files .scratch/` identificar qué hay ahí que SÍ está
   trackeado a pesar del gitignore (excepciones deliberadas, como pasa en
   algunos repos con un puñado de `.md` de mapas). Esos archivos no se
   purgan con `rm`: si el usuario decide purgarlos, es `git rm`, y si se
   promueven hay que decidir si el original se borra o se deja como estaba
   (una excepción ya trackeada puede ser intencional).

## El barrido

Listar el contenido de primer nivel de `.scratch/` (carpetas y archivos
sueltos) con señales rápidas para cada uno — fecha de modificación, tamaño,
si está trackeado en git, tipo de contenido (md, screenshot, html, script).
No hace falta abrir todo de entrada: con el nombre y esas señales muchas
veces ya se sabe si es candidato a purgar (un `screenshot-full.png` de hace
tres semanas) o a mirar con más calma (una carpeta con varios `.md` de
research).

Para cada item, en orden (de más señal de "esto es basura" a menos, o en el
orden que el usuario prefiera):

1. Mostrar un resumen breve — qué es, de cuándo, y si hace falta, un extracto
   del contenido (no pegar un archivo de 300KB entero; resumir).
2. Preguntar: **promover**, **purgar**, o **dejar para después** (skip).
   Usar una pregunta por item o agrupar items obviamente relacionados
   (ej. todos los `.png` de una misma carpeta de observación) si el usuario
   prefiere ir más rápido — pero nunca decidir en su nombre.
3. Según la respuesta, ejecutar antes de pasar al siguiente item (no dejar
   todo para el final: si la sesión se corta a la mitad, lo ya decidido
   debe quedar aplicado).

### Promover

El objetivo es que el hallazgo quede en un lugar versionado, no que el
archivo se mueva tal cual. Antes de escribir nada:

- Leer el contenido y extraer el hallazgo real (la conclusión, el dato, la
  decisión) — no todo el proceso que llevó a él.
- Proponer dónde debería vivir en el repo destino, según lo que ya existe:
  un `CONTEXT.md` si es vocabulario o modelo de dominio, un ADR si es una
  decisión difícil de revertir y no obvia sin contexto, un doc en `docs/`
  si es una nota de proceso o hallazgo de producto, o un archivo de test/
  código si el contenido es en realidad una herramienta reutilizable (un
  script de análisis, por ejemplo).
- Mostrar el texto propuesto (o el diff, si es una sección nueva de un doc
  existente) y preguntar antes de escribir. No asumir que el usuario quiere
  el hallazgo completo promovido tal cual — a veces solo vale una línea.
- Después de escribir, borrar o mover el original de `.scratch/` (si el
  usuario no dice lo contrario) para que no quede duplicado — la fuente de
  verdad pasa a ser el doc versionado.

### Purgar

- Confirmar antes de borrar, mostrando qué se va a borrar (ruta, tamaño).
- Si el archivo está trackeado en git (ver paso de "antes de empezar"),
  usar `git rm` en vez de `rm` para que quede reflejado en el próximo commit.
- Si es una carpeta completa, confirmar que se revisó todo su contenido
  relevante antes de borrarla entera — no purgar una carpeta con `.md` sin
  haberlos mirado solo porque también tiene screenshots viejos.

### Dejar para después

Registrar cuáles quedaron sin decisión (para que la próxima pasada empiece
por ahí) y seguir. No es necesario dejar un archivo de tracking dedicado —
basta con mencionarlo en el resumen final.

## Cierre

Al terminar el barrido (o cuando el usuario corte la sesión), resumir en
tres listas: qué se promovió y a dónde, qué se purgó, y qué quedó pendiente.
Si algo promovido implica un commit (docs/ADR nuevo, código movido), seguir
el flujo normal de commits del repo — no comitear sin que el usuario lo pida.
