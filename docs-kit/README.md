# docs-kit v0.1.0

Paquete portable: **cómo organizar docs** (taxonomía carpeta ↔ tipo de pregunta).

Fuente viva: este directorio en [`agent-scripts`](../README.md).  
Referencia de producción: `doterra/mvp/docs/` (mismo método, con contenido de producto).

## Qué incluye

| Ruta | Rol |
|------|-----|
| [`VERSION`](VERSION) | Semver del kit |
| [`templates/docs/`](templates/docs/) | Árbol mínimo para copiar a un proyecto nuevo |
| [`examples/domain-vs-rfc.md`](examples/domain-vs-rfc.md) | Ejemplo corto: hechos externos vs approach de app |
| [`ADOPT.md`](ADOPT.md) | Cómo adoptarlo en otro proyecto / otra máquina |

## Principio en una frase

| Carpeta | Pregunta |
|---------|----------|
| `domain/` | ¿Cómo funciona el **mundo externo** (negocio, proveedor, reglas reales)? |
| `rfcs/` | ¿Cómo lo **modelamos nosotros** (approach técnico)? |
| `design/` | ¿Qué ve / siente el usuario? (sin código) |
| `architecture/vigente/` | ¿Qué patrón **usa el código hoy**? |
| `architecture/exploration/` | ¿Cómo evoluciona la **forma del sistema** (mapa)? |
| `ADRs/` | ¿Qué quedó **oficial y difícil de revertir**? |
| `ROADMAP.md` | ¿Qué vamos a **ejecutar sí o sí**? |
| `exploration/` | ¿Idea **aún sin validar**? |
| `product/` | ¿Capacidad validada, aún no comprometida? |

`domain/` no habla de MVP ni de implementación. Eso va en ROADMAP / product / RFC.

## Sync entre proyectos

1. El kit vive en `agent-scripts` (o el remote que uses).
2. Cada proyecto declara en su `docs/README.md`: `docs-kit: vX.Y.Z`.
3. Al cambiar el método, subes versión del kit y actualizas consumidores a mano (sin magia).

## Origen

Destilado de `doterra/mvp/docs/README.md` + `rfcs/` + READMEs de architecture/design (2026-07). Cluster «Specs/docs antes de programar» en [`docs/candidates.md`](../docs/candidates.md).
