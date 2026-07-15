# Aviso de brecha en skill

**Regla:** cuando un skill no cubre la tarea completa y creo un archivo o
script efímero para completarla, lo declaro explícitamente — qué skill,
qué le faltó, y si el skill es global (se repetirá en otros repos).

## Sub-reglas

1. **Declarar el parche.** Antes de seguir, decir qué skill se invocó y qué
   parte de la tarea no cubrió — no crear el archivo de relleno en silencio.
2. **Distinguir alcance del skill.** Si vive en `~/.bb/skills` o similar
   (global, todos los repos) marcarlo así; si es local a un repo
   (`<repo>/.claude/skills`), decirlo también.
3. **Si es global, señalar la brecha como recurrente.** Un hueco en un skill
   global probablemente se repite en cualquier otro repo con la misma forma
   de problema (ej. app viva con lógica propia vs. HTML estático) — no basta
   parchear una vez.
4. **No promover el parche a skill sin pedido.** Señalar la brecha es
   obligatorio; ampliar o crear un skill nuevo requiere aprobación explícita
   (ver [`alcance-disciplinado.md`](alcance-disciplinado.md)).

## Por qué

Un parche silencioso resuelve la tarea de hoy pero esconde una brecha que
vuelve a pagarse en el próximo repo. Declararla es lo que permite decidir,
con la información completa, si vale la pena mejorar el skill.

## Origen

Extraída de una conversación en `doterra` (2026-07-15): un skill de
screenshot para HTML estático no cubría una app Svelte con lógica de
captura propia; el agente creó un script Playwright de repro sin avisar
que el skill se había quedado corto.
