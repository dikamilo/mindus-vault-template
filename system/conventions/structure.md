# Structure

Load this when creating, splitting, merging or moving anything in a `hub-tree` layer. `flat` layers have none of this — skip it for `raw/`, `outputs/` and `assets/` and any other `archetype: flat` layer.

## What a hub is

A **hub** is a note tagged with its level's hub tag — `content/hub` by default, set per level in `structure.levels[].tag` — that acts as the map for a region of a layer. Each directory has exactly one hub note, named after the directory. Folder and hub agree, always. Below the layer root there is no `index.md`; the hub note plays that role, with different contents.

## Depth

A layer's `structure.depth` sets the ceiling on nesting, `unbounded` by default. Hubs must earn their existence — depth appears because there is enough material, never because a hierarchy was drawn in advance. Where `depth` is finite and `beyond: free`, structure is enforced for that many levels and then stops entirely: no hub-per-folder rule, no atomicity rule, no frontmatter requirement below it.

`depth: 0` is a legal, degenerate case: an index at the root and nothing enforced below it. It still holds notes subject to the vault's tag and frontmatter rules — only the hub grammar switches off.

## When a hub earns its existence — thresholds

Read the layer's `structure.thresholds` before acting; these are the defaults. Any may be `off`.

| Trigger | Threshold | Action |
|---|---|---|
| Hub has too many direct child notes | `max_children`, 15 | Propose splitting into sub-hubs |
| Sibling notes share an obvious sub-topic | `split_siblings`, 3 | Create a sub-hub and move them |
| A note accumulates inbound links | `promote_backlinks`, 8 | Consider promoting it to a hub |
| A hub has one child and no prose of its own | — | Collapse it into its parent |
| Note exceeds size guidance | `max_words`, 800 | Propose a split into atomic notes |
| New material fits no existing hub | — | Place under nearest ancestor; flag for refactor |

A threshold trip from `create-leaf-hub` or `move-one-level` is autonomous, per `policy.autonomy`; splitting, merging, deleting or renaming an existing hub is always approval-gated, because it touches material that already has a home.

## Hub prose, not lists

A hub note does **not** list its members — that is derivable from the filesystem, the `hubs` frontmatter field, and backlinks. A hub note contains:

- **Orientation** — what this domain is, in two or three sentences.
- **A reading path** — where to start, what to read next, in prose.
- **Tensions and open questions** — where notes disagree, what is unresolved.
- **Links to sub-hubs**, with a sentence on why you would go there.
- **Links to a handful of load-bearing notes** — the ones that matter, not all.

If hub prose starts turning into a bulleted inventory, the hub should be split.

## Layer roots: `index.md`

Every `hub-tree` layer with `structure.index: required` has an `index.md` at its root — the one deliberate exception to "never store what can be derived". Rules:

- Tagged `content/index`. No `hubs` field.
- Lists only **direct children of the layer root** — the top-level hubs, each a wikilink with one line — and never descends further.
- Maintained by whichever skill added, renamed or removed a top-level hub, as part of the same operation, not a later cleanup.
- Never lists notes, only hubs. A note directly at a layer root is a placement failure.

## The `hubs` frontmatter field

- Every note has at least one entry, except layer `index.md` files.
- The **first** entry is the primary hub and must match the note's folder — or, below a layer's enforced depth, the nearest hub above it.
- A **hub note** is the exception that proves the rule: its first entry is the hub of the folder *above* it, not itself. A layer's root index carries no `hubs` at all.
- A **top-level hub** — one directly under a layer root — is the other documented exemption: the "folder above it" is the layer root, which carries an index rather than a hub, and an index is not a valid `hubs` target. Its membership is expressed by the layer `index.md` entry instead.
- Additional entries are cross-cutting claims; the claiming hub should mention the note in its prose, or the link is one-sided.

## Enumeration is computed

No skill maintains a stored list of notes, anywhere but a layer's `index.md`. "What is in this hub?", "what links here?", "which notes are orphans?" are answered by search and backlinks at the moment they are asked — see `system/conventions/tooling.md`.
