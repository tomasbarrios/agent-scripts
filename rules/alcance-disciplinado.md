# Alcance disciplinado (YAGNI)

**Regla:** implementar solo lo aprobado — ítems obligatorios del plan o
pedido explícito del usuario. Todo lo demás se propone, no se codifica.

## Sub-reglas

1. **Solo lo aprobado.** Lo marcado *opcional*, *si trivial* o *considerar*
   no se implementa sin confirmación en ese hilo.
2. **Exploración ≠ spec.** Ideas de exploración (UX, técnica) van a docs,
   no a código, hasta que el usuario pida implementar algo concreto.
3. **Sin comportamiento sorpresa.** Scroll automático, animaciones, toasts,
   undo y similares requieren pedido explícito o ítem obligatorio del plan.
4. **Sin infraestructura especulativa.** No agregar carpetas, scripts,
   hooks, CI ni formatos nuevos sin un dolor concreto y repetido. Preferir
   un archivo + un script mínimo antes que un sistema.
5. **Al cerrar una tarea:** lo opcional útil que surgió se propone como
   backlog en el resumen; nunca se mergea en silencio.
6. **Fuera del repo actual:** leer o escribir en otro checkout o config
   global requiere aprobación explícita previa.

## Por qué

Cada línea no pedida es costo: se revisa, se mantiene y ancla decisiones
futuras. El agente que amplía alcance "porque era fácil" le quita al usuario
la decisión de qué entra al codebase.

## Origen

Extraída de `doterra/AGENTS.md` § YAGNI y § Alcance de implementación, y
`.cursor/rules/alcance-implementacion.mdc` (2026-07-15).
