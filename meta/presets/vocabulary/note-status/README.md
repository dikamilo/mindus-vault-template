# `note-status` vocabulary preset

Adds `status/draft`, `status/stable`, `status/deprecated` to a layer's
accepted tags. Purely descriptive of how finished a note's *writing* is — not
to be confused with a layer's own `lifecycle:` field (e.g. `projects`'
`active`/`paused`/`done`/`archived`), which tracks whether an *item* has
reached a terminal state. A project can be `status/draft` while `active`, or
`status/stable` while `archived` — the two axes are independent.

## Applying it

These tags are yours to apply by hand, in a note's `tags:` list, alongside its
required `content/*` tag. No skill writes, changes or removes one — see design
spec §10.1 on why findings never get written back onto a note.
