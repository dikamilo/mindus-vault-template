---
name: capture
description: Write the user's own material straight into a layer, with no external source to distill. Use when the user dictates a note themselves rather than pointing at something to ingest, or asks to promote a synthesis into a permanent note.
---

# capture

Reads: the user's input.
Writes: any layer granting `capture` write.

## Procedure

1. Ask where the material belongs, if it is not obvious — or resolve it the
   same way `ingest` would, by scope, if it is content bound for an
   `ingest_target` layer.
2. Confirm the target layer grants `capture` write; if none does, say so.
3. Search for an existing note on the same concept first — the same
   extend-beats-create rule as `ingest` applies (`system/system.md`'s hard
   rule 2).
4. Write the note following `system/conventions/notes.md`: correct frontmatter,
   correct `hubs` entry, filed under the right hub.
5. Add cross-links wherever the relationship is real.
6. Update the layer's `index.md` if this created a new top-level hub.
7. Append a `log.md` entry.

## What makes this different from `ingest`

No distillation step, no source to consume, no media extraction — but every
other placement rule still applies. A captured note is not exempt from
frontmatter, hub filing, or linking just because it skipped ingestion.

Promoting a `synthesize` output into a permanent note is a `capture`, not
something `synthesize` does itself — outputs are artifacts, not content, until
a human decides otherwise.
