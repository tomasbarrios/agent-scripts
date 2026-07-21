# Instalar reglas de agente

**"Instalar" acá no significa copiar contenido.** Significa propagar mis
instrucciones a los distintos harnesses de agente (Claude Code, Cursor,
OpenCode) apuntándolos a este repo central, en vez de duplicar texto en
cada lugar. Hay **dos alcances distintos**, con mecanismos distintos:
global (toda la máquina) y por proyecto (un repo con reglas propias).

## Instalación global (toda la máquina)

Para que **cualquier** proyecto en la máquina herede las reglas sin tocar
nada por repo: symlink directo desde la ubicación que cada harness lee por
defecto hacia el `AGENTS.md` central.

**Instalado (2026-07-21) en esta máquina:**

```
~/.claude/CLAUDE.md -> ~/Projects/agent-scripts/AGENTS.md
~/.claude/AGENTS.md -> ~/Projects/agent-scripts/AGENTS.md
```

`~/.codex` no existe en esta máquina (no uso Codex) — ese symlink queda
pendiente para si algún día lo instalo.

El symlink a `~/.claude/CLAUDE.md` es el que importa de verdad: **Claude
Code lee solo `CLAUDE.md` de forma nativa, no `AGENTS.md`** (confirmado
leyendo el repo de referencia, no es un dato de su documentación oficial).
Sin ese symlink puntual, cualquier otro mecanismo de propagación no llega a
Claude Code.

**Consecuencia de tenerlo instalado:** desde ahora, toda sesión de Claude
Code en esta máquina lee `AGENTS.md` (incluido `rules/reforzar.md` por
referencia) como reglas globales, sin importar si el proyecto tiene su
propio `AGENTS.md`/`CLAUDE.md` con la línea puntero o no. La instalación
por proyecto (siguiente sección) sigue haciendo falta para herramientas que
no leen el `CLAUDE.md` global de usuario (OpenCode, Cursor) y para agregar
reglas específicas de un repo por encima de las globales.

## Instalación por proyecto

Para un repo que **ya tiene** su propio `AGENTS.md`/`CLAUDE.md` con reglas
locales (symlinkear el archivo entero pisaría ese contenido):

1. Abrir (o crear) el `AGENTS.md` del proyecto.
2. Como primera línea, agregar el puntero:

   ```
   READ ~/Projects/agent-scripts/AGENTS.MD BEFORE ANYTHING (skip if missing).
   ```

3. Debajo de esa línea, dejar solo las reglas específicas de ese proyecto.
4. Si el proyecto usa un harness que lee `CLAUDE.md` y no `AGENTS.md` (p.
   ej. Claude Code) y no hay symlink global cubriéndolo, repetir el mismo
   puntero como primera línea de `CLAUDE.md`, o symlinkear
   `CLAUDE.md -> AGENTS.md` dentro del proyecto — mismo archivo, cero
   divergencia entre los dos.

No hay traducción por herramienta ni script de instalación: es una línea de
texto que se agrega una vez por proyecto.

## Por qué esta estrategia (y no otra)

Es la que usa **[steipete/agent-scripts](https://github.com/steipete/agent-scripts)**
— fuente copiada: su [`README.md`](https://github.com/steipete/agent-scripts/blob/main/README.md#agent-instructions),
sección "Agent Instructions" (ver también comparación completa en
[`design.md` § 5.8](design.md)): en vez de traducir cada regla a un formato
nativo por herramienta (subagente de Claude Code, rule de Cursor, etc.),
centraliza todo el comportamiento en un único `AGENTS.md`/`AGENTS.MD` en la
raíz del repo central, con symlink para el caso global y puntero de texto
para el caso por-proyecto — elige uno u otro según si el destino tiene o no
contenido propio que preservar.

Gana frente a alternativas más elaboradas (paquete instalable, subagentes
nativos por herramienta) en el caso que más me importa: reflejar en el
repo central un cambio que hice "instalando" en un proyecto no es una
operación aparte — es leer (o, en el caso global, literalmente editar) el
mismo archivo. Nada que sincronizar, nada que se pueda pisar
silenciosamente.

## Qué NO cubre esto

- **Skills:** se instalan por symlink de directorio, no por línea puntero
  (ver [`README.md`](../README.md) y [`skills-hermes.md`](skills-hermes.md)).
- Un script que automatice el paso 2 — no se justifica todavía; agregar la
  línea a mano toma segundos y ocurre pocas veces (una vez por proyecto).
