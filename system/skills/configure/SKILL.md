---
name: configure
description: The setup and customization skill - the only skill that writes meta/. Conversational, no flags or subcommands. Use for first-run setup, adding/removing layers, installing or attaching presets, tuning thresholds and access, editing voice, or checking installation health.
---

# configure

Reads: `meta/vault.yaml`, `meta/presets/`.
Writes: `meta/`, new layer folders, `outputs/configure/`, `log.md`. Also root
files: `README.md` at first run, and repaired symlinks whenever the
installation check runs. Delegates index notes to `scaffold`.

This skill is conversational — there is no flag or subcommand grammar. Match
what the user says against the intent table below; when nothing matches
closely, ask what they want rather than guessing.

## Intent table

| The user says something like | Do this |
|---|---|
| "set up my new vault", or anything while `mindus.name` is still the template default | Run **First run**, below |
| "add a projects folder" / "I want to track projects" | Offer matching presets from `presets/layers/`; install the chosen one |
| "make me a layer for my reading list" | Guided layer authoring (`references/layer.md`); offer to save it as a preset afterwards |
| "I want a second knowledge folder for X" | Add a layer with `ingest_target: true`; ask for its `scope`; set write grants and both sides of its link policy |
| "let me mark notes as draft" | Install and attach `note-status` |
| "my areas notes should use entity and method too" | Attach an already-installed vocabulary to another layer |
| "what's using knowledge-extended?" | List the layers it is attached to |
| "let hubs have 20 children before splitting" | Set that layer's `structure.thresholds.max_children` |
| "the query skill shouldn't look in my journal" | Set `discoverable: false` on that layer |
| "let capture write to areas too" | Add `capture` to that layer's `access.write` |
| "add a tag for book notes" | Guided tag authoring (`references/tag.md`) |
| "change how hub notes sound" | Edit the relevant `meta/voice/*.md` file |
| "turn my areas setup into something reusable" | **Exporting a preset**, below |
| "what can ingest write to?" | Render the transposed access view — read-only, no mutation |
| "is my vault set up correctly?" | **Installation check**, below |
| "remove the library folder" | Refuse if non-empty or `protected`; offer to export it as a preset first |

## First run

Detected by `mindus.name` still holding the template default.

1. Ask three questions: what is this vault called, what language does it hold,
   how should it sound.
2. Set `mindus.name` and `mindus.language`; write `meta/voice/default.md`.
3. Run the installation check (below) and report what it cannot fix.
4. Run `lint` once. A fresh clone must come back clean; if it does not, say so
   plainly — the template itself is broken.
5. Replace the template `README.md` with one describing *this* vault.
6. Append the first `log.md` entry.
7. Mention that `meta/presets/` exists. Install nothing unasked.

Take "not now" for an answer at any point and continue with whatever else was
asked.

## Installing and attaching presets

- **Install** (layer or vocabulary preset): copy `tags/`, `templates/` and any
  `voice/` files into `meta/`; register each template under `templates:` and
  each voice file as a `voice.contexts.<name>` key. Idempotent, touches no
  layer. For a layer preset, also append the layer block to `vault.yaml`,
  create its folder, grant it a `structure` entry naming `scaffold`, then call
  `scaffold` to write its `index.md` — `configure` never writes an index note
  itself.
- **Attach** (vocabulary only, repeatable): record the pairing on both sides —
  the layer's `vocabulary.content` / `vocabulary.tags` /
  `vocabulary.frontmatter.recommended` gains the preset's tags; each tag
  definition's `layers:` field gains the layer's name. If attaching makes no
  sense (e.g. `note-status` onto a `holds: files` layer), warn and proceed only
  if the user still wants it.
- A layer preset declaring `requires: [<vocabulary>]` installs and attaches
  that vocabulary first.
- Show the diff before writing anything.

## Uninstalling

Detach a vocabulary everywhere it is attached; **keep the tags already written
on notes** — do not rewrite them. Count first: *"N notes carry `<tag>`.
Detaching leaves them as-is; they will appear in lint as unknown tags until you
remove them. Continue?"* Never strip notes, never record an exemption.

Refuse to remove a layer that is non-empty or `protected`. Offer to export it
as a preset first.

## Exporting a preset

Write a directory under `meta/presets/layers/` (or `vocabulary/`) with the
layer block parameterised via `preset.prompts`, plus copies of every tag,
template and voice file it references. This is the install path run backwards
— see `references/preset.md`.

## Installation check

Report and, where possible, fix: the four symlinks (`AGENTS.md`, `CLAUDE.md`,
`.agents/skills`, `.claude/skills`) exist and point at `system/`, replacing a
plain-text stand-in on filesystems that cannot symlink; `.obsidian/` carries
`attachmentFolderPath: assets` and the two `userIgnoreFilters` entries; whether
`obsidian-cli` is on the path; whether the working tree is a git repository.

## Safety rails — validate after every mutation

Schema-valid against `system/schema/vault.schema.json`; every declared path
exists; every referenced template, tag and voice file exists; no grant names an
unknown skill; no two layers overlap on `path`; `links.inbound_from` and
`links.outbound_to` agree pairwise; every layer's `vocabulary` and its tags'
`layers:` fields agree; every `checks:` id is in the registry
(`references/checks.md`); exactly one layer per singleton role
(`inbox`/`outputs`/`assets`); no config key nothing reads (`config-unused`).
Removing a layer never deletes its content. Every mutation writes a record to
`outputs/configure/` and is shown as a diff before it is applied.

## Reference files

`references/layer.md`, `tag.md`, `template.md`, `preset.md`, `lifecycle.md` —
guided-authoring detail for each kind of object. `references/checks.md` is the
full lint check registry; `references/values.md` is the full configuration
value reference. Load whichever this conversation actually needs, not all of
them by default.
