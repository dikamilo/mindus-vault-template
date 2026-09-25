---
name: scaffold
description: Create structure inside an existing layer - a new top-level hub, a nested sub-hub, or the root index.md of a layer configure just declared. Never imports or moves material, and never creates a whole new layer. Use for "add a hub for X", or when configure hands off after declaring a layer.
---

# scaffold

Reads: `meta/vault.yaml`.
Writes: folders, hub notes and layer indexes, in layers granting `scaffold` `structure`.

## Procedure

1. Confirm the target layer grants `scaffold` the `structure` verb.
2. Ask for whatever is missing: the name, its parent, a short description.
3. Create the folder and write the hub note from the template its level names — `levels[].folders.<name>.template`, else `levels[].template`, else the layer's `templates.hub` (or, for a brand-new layer's root, `templates.index`) — never freehand. At a level with a closed `folders` set, refuse any name not in it.
4. If the next level declares `required` folders, create each of them under the new hub in the same pass, the same way.
5. If the new thing is top-level, add it to the layer's `index.md` in the same pass.
6. Append a `log.md` entry.

## What this skill does not do

It creates structure only. It never imports material from the inbox (that is `ingest`), never writes atomic notes on its own initiative (that is `ingest` or `capture`), and never creates a new *layer* — declaring a layer, its folder and its `vault.yaml` block is `configure`'s job. `configure` calls `scaffold` only for the resulting `index.md`, once the layer already exists in the config; this keeps every index note written by the same path regardless of how the layer came to exist.
