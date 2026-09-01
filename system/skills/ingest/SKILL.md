---
name: ingest
description: Distill a source from the inbox (or a named file in a readable layer) into new or extended atomic notes. The vault's primary write path. Use when the user drops something into raw/ or says "ingest this" / "add this to the vault".
---

# ingest

Reads: the `inbox` layer, plus any layer granting `ingest` read.
Writes: layers with `ingest_target: true`; the `assets` layer; deletes from
`consumable` layers on success.

## Before anything else

Load `meta/vault.yaml`. Resolve every layer with `ingest_target: true` — there
may be more than one; read each one's `scope`. Confirm
`ingest` holds `write` on a candidate layer before routing anything to it; if it
does not, say so rather than routing there anyway.

## Procedure

1. Read the source — from the inbox, or a named file in a layer that grants
   `ingest` read.
2. **Extract any media to the `assets` layer first.** Nothing may be lost before
   step 10. A source containing diagrams or screenshots must have them saved
   before it is touched again.
3. For each concept the source contains, search *every* `ingest_target` layer
   for an existing note on it. **Extending beats creating** — a near-duplicate
   note is an ingest failure, not a second data point. See
   `system/conventions/tooling.md` for how to search.
4. Route each concept to a layer by its declared `scope`. Routing is **per
   concept, not per source** — one source can legitimately produce notes in more
   than one `ingest_target` layer. Route silently when exactly one scope
   matches; ask when two match or none do; fall back to
   `policy.default_ingest_target` only if the user declines to choose.
5. Capture verbatim quotes for anything you might want to cite later — the
   source will not exist to return to once it is consumed.
6. Write or extend atomic notes in the correct hub, creating sub-hubs where the
   layer's thresholds allow (`system/conventions/structure.md`). Set `title`,
   `tags`, `hubs` on anything new; bump `updated` on anything extended — and
   only for that reason (`system/conventions/notes.md`).
7. Update the prose of every hub the new material affects — new material
   usually changes the reading path, not just the file count. If a new
   top-level hub was created, add it to that layer's `index.md`.
8. Add cross-links in both directions wherever the relationship is real.
9. Append a `log.md` entry naming the source, every layer touched, and every
   note created or extended (`system/conventions/logging.md`).
10. Delete the source from the consumable layer it came from. Never delete
    anything else, and never delete a source that came from a non-consumable
    layer.
11. Report: created, extended, moved, dropped-and-why, and any cross-layer
    routing that occurred.

## Rules specific to this skill

- Duplicate and contradiction detection spans the full set of `ingest_target`
  layers, not just the one you are routing into.
- If no layer's `write` grant includes `ingest`, stop and say so — do not fall
  back to a different layer.
- An inbox partitioned by `raw.partitions` routes without a question; the
  shipped inbox is unpartitioned.
