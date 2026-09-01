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
2. For each concept the source contains, search *every* `ingest_target` layer
   for an existing note on it. **Extending beats creating** — a near-duplicate
   note is an ingest failure, not a second data point. See
   `system/conventions/tooling.md` for how to search.
3. Route each concept to a layer by its declared `scope`. Routing is **per
   concept, not per source** — one source can legitimately produce notes in more
   than one `ingest_target` layer. Route silently when exactly one scope
   matches; ask when two match or none do; fall back to
   `policy.default_ingest_target` only if the user declines to choose.
4. Capture verbatim quotes for anything you might want to cite later — the
   source will not exist to return to once it is consumed.
5. Write or extend atomic notes in the correct hub, creating sub-hubs where the
   layer's thresholds allow (`system/conventions/structure.md`). Set `title`,
   `tags`, `hubs` on anything new; bump `updated` on anything extended — and
   only for that reason (`system/conventions/notes.md`). Handle every image in
   the source as you reach the note(s) it belongs to — see **Images**, below.
6. Update the prose of every hub the new material affects — new material
   usually changes the reading path, not just the file count. If a new
   top-level hub was created, add it to that layer's `index.md`.
7. Add cross-links in both directions wherever the relationship is real.
8. Append a `log.md` entry naming the source, every layer touched, every note
   created or extended, and every image kept, recreated or dropped
   (`system/conventions/logging.md`).
9. Delete the source from the consumable layer it came from — only once every
   image you're keeping has actually been extracted (assets, not the source,
   is now their home). Never delete anything else, and never delete a source
   that came from a non-consumable layer.
10. Report: created, extended, moved, dropped-and-why, what happened to each
    image, and any cross-layer routing that occurred.

## Images

For every image, diagram or screenshot in the source, decide — in the context
of the concept it illustrates — which of three things to do. **Do not extract
by default; extraction is one of three outcomes, not a safety net.**

- **Recreate it as text**, inline in the note it supports, and don't extract
  it at all. A flowchart, architecture diagram or hierarchy usually reads
  better as a Mermaid block (vertical layout preferred — see
  `meta/formatting.md`); a tabular screenshot as a markdown table; a simple
  annotated figure as a sentence of prose. This is almost always the right
  call for a diagram that is really just structured information rendered as a
  picture — recreating it makes that information searchable, diffable, and
  consistent with the rest of the note in a way an embedded image never is.
- **Keep it as-is** when it's not usefully text — a screenshot, a photo, a
  diagram too dense or precise to recreate faithfully. Extract it to the
  `assets` layer and embed it (`![[file.png]]`) in every note that uses it.
- **Ignore it** when it's decorative or carries nothing relevant to any
  concept being distilled — a banner, a logo, a stock photo.

**Only an image you decide to keep gets written to `assets` — never save one
nothing embeds.** An ignored or recreated image is a deliberate distillation
choice, the same as leaving a sentence out of a note; it is not the kind of
loss the vault guards against. The one failure mode to avoid is the opposite:
deciding to keep an image and then deleting the source before it's actually
extracted.

## Rules specific to this skill

- Duplicate and contradiction detection spans the full set of `ingest_target`
  layers, not just the one you are routing into.
- If no layer's `write` grant includes `ingest`, stop and say so — do not fall
  back to a different layer.
- An inbox partitioned by `raw.partitions` routes without a question; the
  shipped inbox is unpartitioned.
- `assets` should end up with no orphans from `ingest`'s own work — an image
  written there should always be embedded by the note it was kept for, in the
  same pass. `orphan-assets` in lint is a backstop for material added by hand
  or by `capture`, not something `ingest` should ever trip.
