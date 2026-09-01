---
name: review
description: The maintenance conversation - walks open findings from recent lint worklists, resurfaces stale notes, and records triage decisions in the log. Writes no content, ever. Use for "let's go through the lint findings", "what needs my attention", or periodic maintenance check-ins.
---

# review

Reads: `discoverable` layers, recent worklists in `outputs/lint/`, `log.md`.
Writes: `outputs/review/` and `log.md` only — **never content**.

## Procedure

1. Gather open (unticked) findings from recent lint worklists, and notes whose
   `updated` (or last `Review: confirmed` record) falls outside
   `policy.staleness_window`.
2. Walk them with the user, one at a time.
3. For each finding, record exactly one outcome:
   - **dismissed** — not a problem. Suppressed while the named notes stay
     unchanged.
   - **deferred** — not now. Suppressed until a stated date.
   - **accepted** — a real problem, to be fixed separately by whoever holds the
     relevant grant. Stays open; no suppression.
   There is no "fixed" outcome here — `review` cannot fix anything. A finding
   only stops appearing once whatever actually changed the vault (an `ingest`
   extend, an approved `refactor`) removes the underlying condition.
4. For each stale note the user confirms is still accurate, append a
   `Review: confirmed` record — this satisfies `stale-note` on the next lint
   run without touching the note's own `updated` field.
5. Append every decision to `log.md` using the exact grammar in
   `system/conventions/logging.md` — do not reword a record line.
6. Optionally write a summary to `outputs/review/`, tagged `output/review`.

## Rules specific to this skill

- `review` never edits a note, never repairs a finding, and never writes to any
  content layer. Its only outputs are log records and, optionally, its own
  summary.
- `worklist-abandoned` exists to catch **accepted** findings nobody ever acted
  on — review does not chase that itself, but should surface it as a topic
  when triaging.
