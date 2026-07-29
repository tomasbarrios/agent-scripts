# Reglas globales → agent-scripts

**Regla:** si una regla, método o convención debe aplicar a **más de un repo** (o a cómo organizas todos tus proyectos), no la inventes ni la “cierres” solo en el repo de producto. **Consulta** para crearla o modificarla en `agent-scripts` (este repo).

## Qué cuenta como global

- Reglas de agente que quieres en todos los repos (`AGENTS.md` central, `rules/`).
- Método de documentación portable (`docs-kit/`).
- Skills compartidas (`skills/`).
- Cualquier “así trabajamos siempre” que no sea específico de un dominio/producto.

## Qué no es global

- Hechos de negocio de un producto (`domain/` del proyecto).
- Approach técnico de una capacidad de ese producto (`rfcs/` del proyecto).
- ADR, ROADMAP, codestyle de un stack concreto del proyecto.

## Cómo actuar (agente o humano)

1. Detectas que la decisión es global (o dudas) → **preguntar** antes de escribirla en el repo de producto.
2. Si se aprueba → editar `agent-scripts` (`rules/`, `docs-kit/`, `AGENTS.md`, skill, …).
3. Si aplica a docs-kit: bump `docs-kit/VERSION` + `CHANGELOG.md`; los consumidores migran después y actualizan su línea `docs-kit: v…`.
4. El repo de producto solo **apunta** o adopta (READ, versión del kit, symlink de skill) — no redefine el método.

## Anti-patrón

Renombrar carpetas canónicas o inventar política multi-repo solo en doterra (u otro producto) y esperar que el central “se entere”. No se entera: diverge.

## Relación con otras reglas

- Declaración corta aquí / en `AGENTS.md`; explicación extendida en este archivo: [`declaracion-vs-explicacion.md`](declaracion-vs-explicacion.md).
- Adopción y sync del kit: [`../docs-kit/ADOPT.md`](../docs-kit/ADOPT.md).
