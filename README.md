# Mindus vault template

This is a [Mindus](https://github.com/dikamilo/mindus-spec) vault: a personal
knowledge base built on plain markdown in Obsidian, maintained primarily by an
LLM agent, and queryable by both a human and an agent.

It is empty of knowledge and complete in structure — conformant to the
[design specification](https://github.com/dikamilo/mindus-spec) on this first
commit. You can drop a file into `raw/` and say "ingest this" before running
any setup.

## Getting started

1. Clone this repository.
2. Open it as a vault in Obsidian (optional, but the point of the format).
3. Talk to your agent of choice — Claude Code, or anything reading `AGENTS.md`
   — and say something like "set up my new vault". The `configure` skill will
   ask a few questions, run an installation check, and replace this file with
   one describing your vault specifically.

## How it's organised

- `raw/` — drop sources here to be distilled (`ingest`), or ask the agent to
  write something directly (`capture`).
- `knowledge/` — the vault's knowledge, organised into nested hubs.
- `outputs/` — answers, syntheses, lint reports and proposals the agent
  writes, never content itself.
- `assets/` — images and other media.
- `meta/` — this vault's own configuration, vocabulary, templates and voice.
  Yours to edit, mostly through `configure`.
- `system/` — the agent's operating instructions: `system.md` (also reachable
  as `AGENTS.md` / `CLAUDE.md`), conventions, and the nine skills.

See `meta/presets/` for optional layers (projects, areas, a permanent library,
a journal) and vocabulary (extended content types, note status, note flags)
you can add as you need them.

## Design

This template implements the [Mindus specification](https://github.com/dikamilo/mindus-spec).
That document holds the rationale for every rule here; this vault holds the
rules themselves. If you're wondering *why* the vault works a certain way,
that's where to look.
