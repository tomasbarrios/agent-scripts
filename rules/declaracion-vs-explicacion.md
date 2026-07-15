# Declaración vs explicación

**Regla:** la regla se declara corta en el archivo que la herramienta lee
(`AGENTS.md`, `.cursor/rules/*.mdc`, `SKILL.md`). La explicación —el porqué,
los ejemplos, los matices— vive en un documento visible para humanos, y la
declaración apunta ahí con una línea: `Fuente de verdad: <ruta>`.

## Por qué

- Si es valioso para un humano, no debe estar en un archivo oculto.
- Cada herramienta necesita su archivo propio, pero la lógica se escribe una
  sola vez; los archivos de herramienta son punteros, no copias.
- Evita divergencia: al cambiar el criterio se edita un solo documento.

## Cómo aplicarla

1. Al crear una regla, escribir primero la explicación en el doc visible
   (README o doc de proceso más cercano al tema).
2. Luego declarar la versión corta (3-10 líneas) en el archivo de la
   herramienta, con el puntero a la fuente.
3. Si una declaración crece más allá de ~10 líneas, es señal de que la
   explicación está en el lugar equivocado.

## Dónde vive la explicación

- **Regla específica de un proyecto:** explicación en los docs de ese repo
  (ej. doterra: `.mdc` → `mvp/docs/devs/...`). No se centraliza.
- **Regla agnóstica (mía, multi-repo):** explicación aquí, en
  `rules/<regla>.md` — este archivo es la explicación. La declaración en
  cada repo apunta con ruta absoluta (`~/Projects/agent-scripts/rules/...`);
  cuando este repo tenga remote, el puntero pasa a ser la URL de GitHub para
  que sea visible desde cualquier máquina o colaborador.

## Ejemplo en producción

Las 5 reglas de `doterra/.cursor/rules/*.mdc` siguen este patrón: cada una
declara en pocas líneas y apunta a `mvp/docs/devs/...` como fuente extendida.

## Pendiente

- Investigar eficiencia de tokens de este patrón (declarado en candidates.md).
