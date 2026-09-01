# Logging

Load this when appending to `log.md`, or reading it back to check dismissals,
deferrals and confirmations. This file's grammar is a contract — do not
improvise it, and do not reword an existing record line.

## Shape of the file

Append-only, newest first, grouped by ISO date under a level-2 heading. Because
notes carry no `sources` field, `log.md` is the vault's only source-level audit
trail, so ingest entries must name the source explicitly. `policy.log_rollover`
moves closed years to `log/<year>.md`; both `lint` and `review` read `log.md`
and every `log/*.md` file.

Two classes of entry live under each day's heading:

```markdown
## 2026-08-19

* **Ingest**: `raw/asyncio-deep-dive.md` (J. Doe, https://example.com/…) →
  created [[event-loop]], [[coroutines-vs-threads]]; extended [[the-gil]];
  created sub-hub [[asyncio]]. Routed to `knowledge`. Source deleted.
* **Scaffold**: created top-level hub [[systems-design]] in `knowledge`.
* **Refactor**: split [[programming]] into [[python]] and [[typescript]] (approved).
* **Configure**: attached `note-status` to `journal`.

* **Triage**: dismissed `duplicates#a3f21c9e` 2026-08-19 — mechanism vs history,
  separate on purpose.
* **Triage**: deferred `contradictions#9d40fe1b` 2026-08-19 until 2026-11-01 —
  revisit after the concurrency material lands.
* **Triage**: accepted `orphans#7b91e0d4` 2026-08-19 — needs a link from [[typescript]].
* **Review**: confirmed [[progressive-overload]] 2026-08-19 — still accurate.
```

## Prose entries

Free text, for humans, never parsed by any skill. Say what was created,
extended, moved, deleted or decided, and — for `Ingest` — name the source.

## Record entries — fixed grammar

```
* **<Kind>**: <verb> <subject> <date> [until <date>] — <prose>
```

where `<subject>` is a backticked finding id or a wikilink, dates are ISO
(`YYYY-MM-DD`), and everything after ` — ` is free text no skill parses. The
date is always inline, even under the matching day heading — a record must be
self-contained.

## Finding ids

`<check-id>#<hash8>` — an 8-character hash over the check id plus the sorted
wikilink targets in the finding. The id is opaque and exists only so a log line
and a report line match mechanically; what a human reads is the sentence and the
wikilinks beside it. Renaming a note changes its wikilink target and therefore
the hash, so a rename invalidates any dismissal or deferral naming the old id —
this is expected, not a bug: the basis moved.

## Read-back regexes

`lint` and `review` run both patterns over `log.md` and `log/*.md`:

```
^\* \*\*Triage\*\*: (dismissed|deferred|accepted) `([a-z-]+#[0-9a-f]{8})` (\d{4}-\d{2}-\d{2})( until (\d{4}-\d{2}-\d{2}))?
^\* \*\*Review\*\*: confirmed \[\[([^\]]+)\]\] (\d{4}-\d{2}-\d{2})
```

`lint` uses the triage matches to suppress dismissed/deferred findings (checking
each dismissal's date against the `updated` of every note the finding names —
if any is newer, the dismissal lapses) and the confirmation matches to satisfy
`stale-note` even when a note's own `updated` is old. `review` uses both to know
what it has already told the user, so it does not re-ask a settled question.
