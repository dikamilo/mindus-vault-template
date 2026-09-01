---
title: output/review
tags: [meta/tag-definition]
tag: output/review
kind: output
layers: [outputs]
frontmatter:
  required:    [title, tags, date]
  recommended: [description]
  optional:    []
template: output
---

# output/review

A record of a maintenance conversation, written by `review` to `outputs/review/`, retained 365 days. Captures which open findings were dismissed, deferred or accepted, and which stale notes were confirmed still accurate — the actual decisions are also appended to `log.md` as `Triage:` and `Review:` records, since that is what later `lint` runs read back. `review` writes no content and modifies no note.
