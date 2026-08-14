# Fuentes externas de skills

**Regla:** cuando se pida instalar una skill, el agente **puede buscarla** en
las fuentes externas declaradas abajo, pero **no instala nada sin aprobación
explícita**. Primero muestra qué encontró (fuente, ruta, qué hace, qué
depende) y espera el OK.

## Fuentes declaradas

| Fuente | Cómo se accede | Qué aporta |
|---|---|---|
| [steipete/agent-scripts](https://github.com/steipete/agent-scripts) | Clon hermano `../agent-scripts-stein` | Estrategia de distribución (symlink global, línea READ) y skills propias |
| [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | Clon hermano `../agent-skills-addy` | Anatomía de skills, `agents/` como unidad instalable |
| [mattpocock/skills](https://github.com/mattpocock/skills) | Submódulo `vendor/mattpocock-skills` (dentro de este repo) | Skills de ingeniería "real" (planificación, revisión, TDD), pensadas para adaptarse |

Las dos primeras se asumen clonadas como **hermanas** de `agent-scripts` en
el filesystem — si el clon no existe, avisar, no clonar por cuenta propia.
`mattpocock/skills` es distinto: vive como **submódulo git** de este repo en
`vendor/mattpocock-skills`, así que siempre está presente tras `git clone
--recurse-submodules` (o `git submodule update --init`). El submódulo fija
un commit — no se actualiza solo; correr `git submodule update --remote
vendor/mattpocock-skills` cuando se quiera avanzar, y confirmar el bump de
commit en un commit propio.

## Por qué requiere aprobación

Instalar una skill de terceros no es copiar un archivo: entra al set que se
carga en **todos** mis repos, compite por nombre con las propias, puede
arrastrar dependencias (otras skills, comandos de setup, herramientas
externas) y su descripción define cuándo se dispara sola. Es una decisión de
alcance, no un detalle de implementación — la toma Tomás.

## Cómo actuar (agente)

1. **Buscar** en los clones locales y en `vendor/mattpocock-skills` y
   reportar: nombre, fuente, ruta, resumen de una línea, dependencias
   detectadas.
2. **Proponer modo de instalación** y sus consecuencias:
   - **Symlink** a la ruta del clon upstream → se actualiza con `git pull` en
     ese repo, pero editarla es editar el clon de otro (no versionado acá).
   - **Copia (vendor)** dentro de `skills/` → queda versionada acá y se puede
     adaptar, pero no se actualiza sola.
3. **Esperar aprobación explícita.** Sin OK, no se crea ni symlink ni copia.
4. Al instalar, dejar **procedencia** en la skill: repo, ruta upstream,
   commit y fecha. Sin eso se pierde de dónde vino y qué se adaptó.
5. Registrar la skill en el catálogo [`../docs/skills-hermes.md`](../docs/skills-hermes.md),
   con su nivel de portabilidad y notas por cliente si aplica.

## Anti-patrón

Ver una skill que "obviamente sirve" en una fuente externa y copiarla
mientras se hace otra cosa. Aunque sea buena, la instalación es un cambio al
sistema global de Tomás — se propone, no se ejecuta.

## Relación con otras reglas

- Alcance: instalar sin pedir es infraestructura no aprobada —
  [`alcance-disciplinado.md`](alcance-disciplinado.md).
- Cualquier skill nueva es global por definición —
  [`reglas-globales.md`](reglas-globales.md).
- Distribución (symlink por cliente) — [`../docs/skills-hermes.md`](../docs/skills-hermes.md),
  comparativa de estrategias en [`../docs/design.md`](../docs/design.md) § 5.
