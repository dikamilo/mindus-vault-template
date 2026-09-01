# Formatting

The vault must be readable without tooling — plain markdown, no required
plugin, everywhere by default.

- **Wikilinks** (`[[note-name]]`, `[[note-name|display text]]`) and **embeds**
  (`![[file.png]]`) are the one deliberate departure from plain-markdown-only:
  they are core Obsidian syntax, not a plugin, and the vault is an Obsidian
  vault first.
- **No Dataview, Bases, Templater, or other plugin syntax** in a note, hub,
  index or output unless the user has explicitly asked for it. Where
  `synthesize` offers a live Dataview/Bases view instead of a static table, it
  says so in the output and states that the plugin is required — the default
  stays a static markdown table.
- **Tables** for structured comparisons; plain markdown, no plugin needed.
- **Blockquotes** (`>`) for verbatim quotes, always attributed.
- **Callouts** (`> [!note]`, `> [!warning]`, `> [!question]`, `> [!example]`,
  …) are core Obsidian syntax, not a plugin — use them where a block genuinely
  wants visual separation from surrounding prose: `[!question]` or
  `[!warning]` for a hub's unresolved tensions and open questions,
  `[!example]` for a worked example inside a concept note, `[!info]` for a
  supporting aside that would otherwise interrupt the main claim. Don't box up
  the note's central claim in a callout — that belongs in plain prose, read
  first, not set apart.
- **Mermaid diagrams** (` ```mermaid `) for a relationship, process or
  hierarchy that is genuinely structural — recreated from a source image at
  ingest time (see `system/skills/ingest/SKILL.md`), or built directly where
  prose alone would blur the shape. Core Obsidian rendering, not a plugin.
  **Prefer a vertical layout** (`graph TD` / `flowchart TD`) over a horizontal
  one (`graph LR`) by default — a vertical diagram reads top-to-bottom the way
  the surrounding note does, and doesn't get clipped by note width the way a
  wide horizontal one can. Use `LR` only when the relationship is genuinely
  a left-to-right sequence and forcing it vertical would distort it.
- **Checkboxes** (`- [ ]` / `- [x]`) for lint findings only — that is the one
  place the vault relies on them being interactive, since Obsidian renders
  them as clickable natively.
- **Headings** start at `#` for the note's title, `##` for sections. Don't
  nest past `###` in an atomic note — if you need to, the note has stopped
  being atomic.
- **No custom CSS classes, callout types beyond Obsidian's built-in set, or
  inline HTML**, unless asked for.
