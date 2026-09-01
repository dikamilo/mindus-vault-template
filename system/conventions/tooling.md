# Tooling

Load this when searching, enumerating, or answering "what is in this hub / what links here / is this an orphan". The rule behind all of it: **enumeration is always computed, never stored.** No skill maintains a list of notes anywhere but a layer's `index.md` (top-level hubs only).

## `obsidian-cli`

Prefer `obsidian-cli` over ad-hoc file reads when it can answer the question directly — it resolves wikilinks and backlinks the way Obsidian does, which a plain grep over paths does not.

| Question | Command shape |
|---|---|
| Open or read a note by name | `obsidian-cli open "<note-name>"` |
| Full-text / frontmatter search | `obsidian-cli search "<query>"` |
| What links to this note | `obsidian-cli backlinks "<note-name>"` |
| List notes carrying a tag | `obsidian-cli search "tag:<content/concept>"` |

Where `obsidian-cli` is not installed or does not cover the case, fall back to `rg` over the vault, matching on frontmatter (`tags:`, `hubs:`) or on `[[wikilink]]` syntax in bodies. `configure`'s installation check reports whether `obsidian-cli` is present.

## Answering the standard questions

| Question | Mechanism |
|---|---|
| Members of a hub | Directory listing, plus a frontmatter search on `hubs` |
| What references this note | `obsidian-cli backlinks` / Obsidian's backlink pane |
| Notes matching a topic | `obsidian-cli search` / `rg` over frontmatter and body |
| Notes of a kind | Tag search (`content/concept`, `output/query`, …) |
| Orphans | Zero backlinks *and* unmentioned in any hub's prose — both, not either |

None of these answers are ever cached in a note or a hub. If you find yourself about to write "members: [...]" or "notes in this hub:" anywhere but a layer's `index.md`, stop — compute it instead.
