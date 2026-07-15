# Plan: tres primeros pasos hacia el repo centralizado

Basado en [`candidates.md`](candidates.md) (notas de Tomás, 2026-07-14) y
[`design.md`](design.md). Cada paso cabe en una sesión corta, tiene
**una sola decisión** que tomar (con recomendación), y un resultado
verificable. El orden importa: cada paso usa el resultado del anterior.

---

## Paso 1 — Fundar la estructura mínima con el meta-patrón como primera regla

**Qué:** crear la estructura de carpetas del repo y centralizar la primera
regla: el meta-patrón "declaración vs explicación" (cluster 4). Se elige esta
y no una nota-10 porque define **cómo se escriben todas las demás** — sin
ella, los pasos 2 y 3 no tienen formato al cual apegarse.

**Entregable:**

```
agent-scripts/
├── README.md          ← enrutador (aplica tu propia regla de cluster 3)
├── rules/
│   ├── README.md      ← índice: qué reglas hay y cuándo aplican
│   └── declaracion-vs-explicacion.md   ← la regla + su explicación
├── candidates.md      (ya existe)
└── docs/design.md     (ya existe)
```

**Decisión única:** ¿un archivo por regla en `rules/` (recomendado — permite
apuntar a cada regla individualmente desde los repos consumidores) o un solo
`AGENTS.md` monolítico estilo steipete (más simple, pero repite el problema
de doterra: secciones duplicadas dentro de un archivo grande)?

**Esfuerzo:** ~30 min. Todo el contenido ya existe — es redacción, no diseño.

**Verificación:** la regla escrita se explica a sí misma usando su propio
formato (regla corta arriba, explicación y porqué abajo).

---

## Paso 2 — Promover dos clusters nota-10 redactados en forma agnóstica

**Qué:** extraer de doterra y reescribir sin referencias al proyecto los dos
clusters más textuales y portables:

1. **Alcance disciplinado / YAGNI** (cluster 5) → `rules/alcance-disciplinado.md`.
   Bonus inmediato: al centralizarla se elimina la sección duplicada dos veces
   en el `AGENTS.md` de doterra.
2. **Comunicación pedagógica** (cluster 6) → `rules/comunicacion-pedagogica.md`
   (humano primero / Antes-Después-Porqué / explicar el porqué). Tu nota
   sobre el cluster 10 se aplica aquí: la skill `css-debug` pertenece a este
   tema — la regla la menciona como implementación ejemplar, sin moverla aún.

**Por qué estos dos y no los cinco nota-10:** etapas MIW/MIR/MIF (1),
specs-first (2) y documentación continua (8) dependen de decisiones de
taxonomía documental que conviene tomar con calma; estos dos son copy-edit
casi puro. Regla del plan: **una regla por sesión, no vaciar doterra de una
vez.**

**Decisión única:** ¿las reglas centralizadas se escriben en español (como
hoy en doterra, recomendado — es tu idioma de trabajo) o en inglés (mejor si
algún día compartes el repo)? Decidirlo ahora evita reescribir después.

**Esfuerzo:** ~45 min por regla. Es extracción + poda de referencias a
doterra, siguiendo el formato del paso 1.

**Verificación:** cada regla se lee completa sin necesitar contexto de
doterra — prueba: dársela a un agente en un repo distinto y que la aplique.

---

## Paso 3 — Resolver la distribución con un caso de prueba real

**Qué:** cerrar la pregunta abierta ("¿qué pasa en el repo origen al
centralizar?") no en abstracto sino con los dos casos concretos que ya
existen, uno por dirección:

1. **Skill duplicada:** `orchestrate-opencode` vive en `~/.bb/skills/` y en
   `doterra/.bb/skills/`. Hacer `diff` de ambas, elegir la canónica, moverla
   a `skills/orchestrate-opencode/` en este repo, y dejar las otras dos
   ubicaciones como symlink hacia acá.
2. **Regla centralizada:** en el `AGENTS.md` de doterra, reemplazar las
   secciones ya promovidas en el paso 2 por una línea puntero
   ("Alcance: leer `~/Projects/agent-scripts/rules/alcance-disciplinado.md`"),
   estilo steipete.

**Decisión única:** ¿symlink (recomendado por design.md § 5.2-5.3: editar la
copia instalada ES editar la central, cero divergencia) o copia + puntero en
texto? Si algún entorno tuyo no soporta symlinks, se cae a puntero — pero
pruébalo primero con este caso antes de generalizar.

**Esfuerzo:** ~30 min. Un diff, un `mv`, dos `ln -s`, una edición de AGENTS.md.

**Verificación:** en doterra, invocar la skill y pedir a un agente que aplique
la regla de alcance — ambas deben funcionar leyendo desde el repo central.
Editar la skill desde doterra y confirmar que el cambio aparece en
`git status` de agent-scripts.

---

## Qué queda explícitamente FUERA de estos tres pasos (YAGNI aplicado)

- Script de sync automático o periódico — recién se justifica cuando el
  ritual manual haya corrido 2-3 veces y duela.
- Promover los clusters 1, 2 y 8 (nota 10 pero requieren diseño de taxonomía).
- El cluster 13 (narrativas) — pendiente de tu evaluación en candidates.md.
- La herramienta de auditoría "qué contenido va en qué tipo de archivo".
- Soporte Cursor/OpenCode más allá de lo que el symlink ya resuelve.

Al completar los tres pasos: actualizar la columna "Cobertura central" en
[`candidates.md`](candidates.md) — sería la segunda pasada del escaneo, la
primera donde esa columna mide algo real.
