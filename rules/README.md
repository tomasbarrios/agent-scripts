# rules/

Reglas consolidadas, agnósticas al proyecto. Un archivo por regla:
declaración corta arriba, explicación abajo. Los repos consumidores apuntan
acá desde sus archivos de herramienta (`AGENTS.md`, `.mdc`).

| Regla | Cuándo aplica |
|-------|---------------|
| [declaracion-vs-explicacion.md](declaracion-vs-explicacion.md) | Al crear o editar cualquier regla, skill o hook |
| [alcance-disciplinado.md](alcance-disciplinado.md) | Al implementar cualquier plan o pedido |
| [comunicacion-pedagogica.md](comunicacion-pedagogica.md) | Al redactar respuestas, reviews, commits o docs |
| [aviso-brecha-skill.md](aviso-brecha-skill.md) | Al crear un archivo/script para completar lo que un skill no cubrió |
| [autoria-de-ideas.md](autoria-de-ideas.md) | Al listar ideas, propuestas o alternativas junto a las del usuario |
| [escalera-de-exploracion.md](escalera-de-exploracion.md) | Ante un objetivo ambicioso de forma incierta, antes de construir |
| [techlead-review.md](techlead-review.md) | Al revisar código, ADRs, planes o arquitectura (pide el usuario) |
| [reglas-globales.md](reglas-globales.md) | Antes de crear/modificar una regla o método que deba aplicar a más de un repo |
| [fuentes-externas-de-skills.md](fuentes-externas-de-skills.md) | Al pedir instalar una skill: dónde buscarla y por qué requiere aprobación |

## ¿Regla (AGENTS) o skill?

- **Regla → AGENTS.md/.mdc:** restricción de comportamiento que debe regir
  en toda conversación sin que nadie la invoque (alcance, comunicación).
  Siempre en contexto → declaración corta + puntero acá.
- **Skill:** procedimiento con pasos para un tipo de tarea puntual
  (debuggear CSS, procesar inbox). Se carga bajo demanda.
- Los archivos de esta carpeta son la fuente de verdad; el empaquetado
  (línea en AGENTS, `.mdc`, skill) lo decide cada consumidor.

Las reglas aún en adquisición viven en [`reforzar.md`](reforzar.md).
Las candidatas sin promover, en [`../docs/candidates.md`](../docs/candidates.md).
