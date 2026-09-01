# Mindus vault — agent instructions

This is a personal knowledge vault. Knowledge is distilled from sources into small atomic notes, organized into nested hubs, and maintained by you.

## Before anything else

Read `meta/vault.yaml`. It is the authoritative description of this vault: which layers exist, what each holds, which skills may read or write them, and which structural rules apply where. This file describes *how* to operate; the config describes *what exists*. Where they disagree, the config wins — and say so, because it means this file needs updating.

Never infer the layout from a directory listing. A folder that is not in `vault.yaml` is not a layer, and writing into it is an error.

## Hard rules

1. Check the config before writing. If no layer grants you write access for what you are about to do, stop and say so. Never fall back to a default.
2. Before creating a note, search for an existing one across every layer with `ingest_target: true`. **Extend beats create.**
3. Never write a list of notes into a hub or a note. Compute it. The only stored list is each layer's `index.md`, which lists top-level hubs only — update it whenever you add, rename or remove one.
4. Deleting a source from the inbox after a successful ingest is the one routine deletion. Nothing else is ever deleted as a side effect. A requested deletion needs the target layer's `delete` grant and your confirmation; if no skill holds that grant, say so instead of working around it.
5. For every image in a source, decide before consuming it: recreate it as text (a Mermaid diagram, a markdown table, prose) inside the note it supports, keep it as-is by extracting it to the assets layer and embedding it, or ignore it as decorative. Only images you decide to keep are written to assets — never save one nothing embeds. Make the decision before the source is deleted; a kept image not yet extracted is lost the moment the source goes.
6. Capture verbatim quotes at ingest — the source will be gone.
7. Every note needs the frontmatter its layer requires. In a knowledge-shaped layer that means `title`, one `content/*` tag from its vocabulary, and `hubs` — except layer `index.md` files and material below the enforced depth. Layers declaring their own requirements, such as outputs, follow those instead.
8. Write each paragraph, bullet and blockquote as one line in the file, no matter how long, and let the editor soft-wrap it. Do not hard-wrap prose at a fixed column the way this file does — that convention is for instructions read as source, never for content you write. See `meta/formatting.md`.
9. English everywhere, except the material in layers declaring `language: any` — whose index and hub notes are still English. Translate during ingest.
10. Never modify `system/` as a side effect of anything. If asked directly, say what you are changing.
11. Log what you did, before you finish.

## Load when you need it

| When doing | Load |
|---|---|
| Any structural change | `system/conventions/structure.md` |
| Writing or editing a note | `system/conventions/notes.md` |
| Searching or enumerating | `system/conventions/tooling.md` |
| Writing or reading log records | `system/conventions/logging.md` |
| Anything approval-gated | `policy.autonomy` in the config |
| Choosing a content type | `meta/tags/<type>.md` |
| Shaping prose | `meta/formatting.md`, `meta/voice/` |
| A specific operation | that skill's `SKILL.md` |

## Skills

`ingest` — distill a source from the inbox into notes. The main write path.
`capture` — write your own material straight into a layer, no source.
`scaffold` — create a hub or folder inside an existing layer.
`query` — answer a question from the vault; writes to outputs.
`synthesize` — derive comparisons and distillations; writes to outputs.
`lint` — health check; writes a triage worklist to outputs.
`refactor` — propose and, on approval, apply restructuring.
`review` — triage findings and resurface stale notes. Writes no content.
`configure` — change the vault's configuration. The only skill that writes `meta/`.

Only `ingest`, `capture`, `scaffold` and `refactor` write notes into a content layer. The six skills holding a write grant on outputs write there — check the config rather than assuming, since `ingest`, `capture` and `scaffold` have no outputs partition at all. And anything may save a file to the inbox or the assets layer, which hold files, not notes.

## Logging

Append to `log.md` — newest first, grouped by ISO day — whatever you created, extended, moved, deleted or decided. Ingest entries must name the source: it is the only record that the source ever existed.

Triage and review records have a fixed grammar; see `system/conventions/logging.md` and do not reword them.

## Autonomy

Create and extend notes, add links, rewrite hub prose, create leaf sub-hubs, save assets, update indexes, consume a source from the inbox after a successful ingest: do it and log it. Splitting or deleting hubs, moving notes across domains, deleting a note or an asset, deleting from any layer that is not the inbox, changing config, or touching more than `bulk_threshold` files: propose first.

## Tooling

Use `obsidian-cli` for search, backlinks and open. Enumeration is always computed, never stored.

## Vault-specific instructions

If `meta/instructions.md` exists, read it. It holds guidance particular to this vault that this file — the same in every Mindus vault — cannot carry.
