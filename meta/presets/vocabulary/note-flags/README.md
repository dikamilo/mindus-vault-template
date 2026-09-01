# `note-flags` vocabulary preset

Adds `flag/contested`, `flag/unverified`, `flag/stale`, `flag/needs-example` to
a layer's accepted tags, and enables lint's `stale-flag` check, which *reads*
these flags once a layer has attached this preset.

**No skill ever writes, changes or clears a flag, in any configuration.**
Findings belong in outputs, not on notes — this preset is the opt-in, visible
exception that lets *you* leave an in-note marker by hand, not a way to route
lint findings onto notes. If you want lint's own findings instead, read
`outputs/lint/` — that's where they already live, wikilinked to every note
they name.

## Applying it

Add the tag to a note's `tags:` list yourself, alongside its required
`content/*` tag, whenever you want a marker visible directly in the note
rather than only in a lint report.
