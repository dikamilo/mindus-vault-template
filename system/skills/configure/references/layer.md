# Authoring a layer

A layer is a region of the vault with a declared shape, vocabulary, permissions and link policy. Every content folder is one. There are exactly two archetypes, and everything the vault has ever needed is one of them with different values — see `references/values.md` for the full field list.

## Choosing an archetype

- **`hub-tree`** — a root `index.md`, one hub note per folder named after the folder, atomic notes below, `hubs` frontmatter, wikilinks. Use for anything that should grow a navigable structure as material accumulates.
- **`flat`** — no hub grammar, no index, files are peers. Use for a queue (`consumable: true`), a dated log of artifacts, or a machine-managed store. `holds: files` switches off the frontmatter, tag and language rules that only make sense for markdown.

`depth: 0, beyond: free` on a `hub-tree` layer is the escape hatch for material that resists structure: an index at the root, unconstrained below it, but still holding notes subject to the vault's tag and frontmatter rules — which is what distinguishes it from a `flat` layer, whose files are not notes at all.

## Minimum viable declaration

Only `path` is required; everything else falls back to `defaults` in `vault.yaml`, then to the defaults in `references/values.md`. In practice, set at minimum:

```yaml
<name>:
  path: <name>/
  archetype: hub-tree            # or flat
  title: <Title>
  description: <one sentence>
  access:
    write:     [<skills>]        # defaults to [] — an unwritable layer otherwise
    structure: [<skills incl. scaffold>]   # scaffold needs this for the index note
    delete:    [<skills>]
```

A layer with no `write` grant refuses every write routed to it; a layer with no `structure` grant cannot even receive its own `index.md`. Set both when creating a layer, and say which you set.

## An `ingest_target` layer

Also set `scope:` — the prose `ingest` routes concepts by — and both sides of `links:`. If this is a second (or later) knowledge-shaped layer, append its name to `inbound_from` and `outbound_to` on every *existing* `ingest_target` sibling too, or `config-link-policy-agrees` will fail. Say explicitly that turning `ingest_target` on also brings this layer into cross-layer duplicate detection — turning it off stops that.

## Four things worth checking before you save

1. **`links.inbound_from` belongs on the target, not the source.** "X must not link into Y" is Y's declaration, not X's.
2. **`discoverable` is separate from `access.read`.** A skill may be able to open a layer without that layer's contents ever being cited as an answer — `raw`, `outputs` and `assets` are all `discoverable: false` for this reason.
3. **`checks:` selects from the registry** (`references/checks.md`) rather than describing new behaviour — a hand-rolled rule has no home here.
4. **The outputs layer is exempt from inbound policy** — do not add `outputs` to every content layer's `inbound_from`; `links-inbound-policy` already ignores links whose source is the `outputs` role.

## After writing the block

Run the safety-rail validation in `SKILL.md` before treating the layer as live: paths exist, templates/tags/voice referenced exist, no path overlap, link policy agrees both directions, vocabulary and tag `layers:` agree. Then create the folder and call `scaffold` for its `index.md` — `configure` never writes that note itself.
