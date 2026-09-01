# Notes

Load this when writing or editing a note in any `holds: notes` layer.

## Frontmatter

Required on every note, unless the layer's `vocabulary.frontmatter.required`
says otherwise:

| Field | Type | Notes |
|---|---|---|
| `title` | string | Human-readable; may differ from filename |
| `tags` | list | A `content/*` tag from the layer's vocabulary — exactly one unless the layer sets `require_exactly_one: false` |
| `hubs` | list of wikilinks | Except layer-root indexes |

Common optional fields:

| Field | Shape | Purpose |
|---|---|---|
| `description` | string | One sentence; lets a skill summarize without reading the note |
| `created` / `updated` | `YYYY-MM-DD` | `updated` means *content changed* and nothing else — never "someone looked at this and it's fine"; that is a log entry |
| `aliases` | list | Obsidian-native; alternate names that resolve to this note |

Everything else is declared per content type in `meta/tags/<type>.md` — read that
file when choosing between two tags or wondering what a type expects.

## Filenames

Kebab-case, matching the concept: `structural-typing.md`, not
`Structural Typing (TS).md`.

## Tags replace `type`

Categorization lives in tags, not a `type` field. Each note carries exactly one
`content/*` tag unless its layer sets `require_exactly_one: false`. Topic
categorization is not a tag namespace — that is what hubs are for.

## Atomicity

One note, one concept. Target length comes from the tag definition —
`content/concept` defaults to 100–500 words. A note past the layer's
`max_words`, or one that acquires two independent H2 sections linking to
different neighbours, is a split candidate.

## Linking

All internal links use Obsidian wikilinks: `[[decorators]]`, with the pipe form
where prose demands it: `[[the-gil|the GIL]]`. Embeds use wikilink form too:
`![[event-loop-diagram.png]]` — Obsidian resolves the name, not the path, so
moving a note never breaks its images.

See `system/conventions/structure.md` for the `hubs` frontmatter field's rules —
which entry is primary, and when a hub note's own `hubs` points above it rather
than at itself.

## No `sources` field

Notes carry no `sources` frontmatter — a note is not owned by the source that
created it, and after several ingests such a list would describe the note's
history rather than any particular claim. Provenance lives in two places
instead:

- **In the body** — inline quotes, attributions in prose, and an optional
  `## References` section with links to originals.
- **In `log.md`** — the source-level audit trail.

```markdown
> "The loop does not preempt. If your coroutine does not await, nothing else in
> the process makes progress." — J. Doe, *A deep dive into asyncio*

## References
- [A deep dive into asyncio](https://example.com/asyncio-deep-dive) — J. Doe
```

## Language

English everywhere — filenames, frontmatter, bodies, hub prose — even when a
source is in another language; translate during ingest. Exceptions: proper nouns
keep their native form; quoted source material may appear verbatim if an English
translation follows; a layer may set `language: any`, exempting its content, but
its layer index and hub notes stay English so navigation behaves consistently.
