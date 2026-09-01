---
name: lint
description: Periodic health check across the vault. Runs each layer's declared checks, reads the log for prior dismissals and deferrals, and writes a dated triage worklist to outputs. Use for "lint the vault", "check for problems", or before a refactor/review pass.
---

# lint

Reads: everything except `excluded` paths (`meta/`, `system/`).
Writes: `outputs/lint/` only.

## Procedure

1. For every layer, run the checks named in its `checks:` list (or the
   `[core]` default) — resolve aliases and severities against the registry in
   `system/skills/configure/references/checks.md` (Appendix C of the design
   spec).
2. Read `log.md` and `log/*.md` back using the regexes in
   `system/conventions/logging.md`. Suppress a finding whose id was dismissed
   or deferred, **unless** any note it names has an `updated` date newer than
   the dismissal — in which case report it again, noting the earlier
   dismissal and why it lapsed. A deferral simply expires past its `until`
   date; no lapse logic needed.
3. Count suppressed findings and report the count — never a silent
   disappearance.
4. Assign each surviving finding a stable id: `<check-id>#<hash8>`, hashed over
   the check id plus the sorted wikilink targets involved.
5. Render the worklist: a `## Findings` section of markdown checkboxes, each
   wikilinking every note it names; a `## Suppressed` section listing
   suppressed ids and why. Follow the shape in
   `system/skills/configure/references/checks.md`'s companion example in the
   design spec (Appendix A.5) — unticked box, id, wikilinks, one-line
   description.
6. Never name a report itself as prunable by `retention` while it has any open
   (unticked) finding, regardless of the outputs partition's retention value.
7. Write to `outputs/lint/`, tagged `output/lint`, grouped by month per that
   partition's `group_by`.
8. Append a `log.md` entry.

## Rules specific to this skill

- A finding never invalidates a note (P5) — lint ranks severities to say what
  to look at first; it never refuses to read or cite a note because of one.
- `lint` only *reports*; it never writes a flag, status or finding onto a note,
  in any configuration — even where `note-flags` is attached, `lint`'s
  `stale-flag` check only *reads* flags a human applied by hand.
- `lint` only reads `log.md` back to suppress; it does not act on past
  judgements beyond that (§10.3 of the design spec) — a vault that let old
  decisions constrain new ingests would get worse at improving over time.
- `duplicates` and `contradictions` compare across every `ingest_target` layer
  as a single set; every other check runs per layer.
