---
name: query
description: Answer a question from the vault's existing content and write the answer to outputs. Never modifies content. Use whenever the user asks a question the vault might already have the answer to.
---

# query

Reads: every layer with `discoverable: true`.
Writes: `outputs/query/` only.

## Procedure

1. Find candidate notes via search, tags and backlinks across every `discoverable` layer (`system/conventions/tooling.md`).
2. Read them.
3. Synthesize an answer, wikilinking `[[every note used]]` in the body — never a `notes_consulted` frontmatter field; the wikilinks are what puts this output in each note's backlink pane.
4. If the vault does not contain the answer, say so explicitly, with a `**Gap:**` line naming what is missing. Do not fill the gap from model priors and present it as vault content.
5. Write the result to `outputs/query/`, tagged `output/query`, following `meta/templates/output.md`.
6. Append a `log.md` entry.

## Rules specific to this skill

`query` never writes to a content layer and never modifies a note it reads — its only side effect is the output it produces. Layers with `discoverable: false` (`raw`, `outputs`, `assets`, and any layer a user has opted out) are out of scope even if `query` otherwise holds read access to them.
