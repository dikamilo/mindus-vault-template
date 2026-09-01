---
name: synthesize
description: Derive new material from existing notes - a comparison, a distillation of a hub, a reconciliation of a contradiction, a generated document. Writes only to outputs, never to content. Use for "compare X and Y", "summarize this hub", "reconcile these notes".
---

# synthesize

Reads: every `discoverable` layer.
Writes: `outputs/synthesize/` only.

## Procedure

1. Gather the relevant notes the same way `query` does.
2. Produce the derived material — comparison table, distillation, generated
   doc — wikilinking every note it draws on.
3. Where a user wants a live view rather than a snapshot, a Dataview or Bases
   block may replace a static table; say in the output that it requires the
   plugin. Default to a static markdown table otherwise — correct the moment
   it is written, and readable without tooling.
4. Write to `outputs/synthesize/`, tagged `output/synthesis`.
5. Append a `log.md` entry.

## Rules specific to this skill

`synthesize` never writes to a content layer — a synthesis is an artifact, not
knowledge, no matter how good it is. If it proves durable, the user promotes it
deliberately via `capture`; nothing does this automatically.
