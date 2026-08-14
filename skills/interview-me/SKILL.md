---
name: interview-me
description: >-
  Interviews the user one question at a time — with a recommended answer each
  time — until shared understanding. Resolves decision-tree branches and
  dependencies before any action. Use only when explicitly invoked
  ("interview me", "grill me").
disable-model-invocation: true
---

> **Nota:** este método viene originalmente de una skill de
> [mattpocock/skills](https://github.com/mattpocock/skills), heredado vía
> [este artículo](https://www.theocharis.dev/blog/llm-critics-are-right-i-use-llms-anyway/).
> La versión de Matt hoy es más elaborada — `grilling`/`grill-me` en
> `vendor/mattpocock-skills/skills/productivity/grilling/`, con rondas por
> "frontera" (solo pregunta lo que ya tiene prerequisitos resueltos) y
> delegación de hechos a sub-agentes en vez de preguntarlos. Si `interview-me`
> se queda corto, instalar esa en vez de expandir esta a mano.

Interview me relentlessly about every aspect of this until we reach a shared understanding. Walk down each branch of the decision tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.

Ask the questions one at a time, waiting for feedback on each question before continuing. Asking multiple questions at once is bewildering.

If a fact can be found by exploring the environment (filesystem, tools, etc.), look it up rather than asking me. The decisions, though, are mine — put each one to me and wait for my answer.

Do not act on it until I confirm we have reached a shared understanding.
