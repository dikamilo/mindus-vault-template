# `library` preset

Permanent primary material — courses, books, decks, exercises — kept for its own sake rather than distilled. Two levels are enforced (shelf, then collection); everything below a collection is free-form, mirroring whatever shape the material arrived in.

`write: []` makes it read-only to every skill: material is added by hand, not by `ingest` or `capture`. `structure: [scaffold]` still allows new shelves and collections to be created on request. `delete: []` means nothing here is ever removed by a skill — which is the point of a layer you keep things in. `language: any` lets primary material stay in its original language; the shelf and collection hubs above it stay English so navigation and search behave consistently across the whole vault.

## When you want this

You have source material worth keeping in full — not just the concepts you'd distill from it — and you want it navigable without forcing it into the vault's usual hub grammar past two levels. If you'd rather extract the ideas and discard the original, that's `ingest` into `knowledge`, not this.

## Producing

```
library/
  index.md                        ← lists the shelves
  brave-education/
    brave-education.md            ← shelf (content/hub)
    ai-devs-4/
      ai-devs-4.md                ← collection (content/collection)
      s01/
        lesson-notes.md
        prompts/…
```
