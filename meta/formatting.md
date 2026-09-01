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
- **Checkboxes** (`- [ ]` / `- [x]`) for lint findings only — that is the one
  place the vault relies on them being interactive, since Obsidian renders
  them as clickable natively.
- **Headings** start at `#` for the note's title, `##` for sections. Don't
  nest past `###` in an atomic note — if you need to, the note has stopped
  being atomic.
- **No custom CSS classes, callout types beyond Obsidian's built-in set, or
  inline HTML**, unless asked for.
