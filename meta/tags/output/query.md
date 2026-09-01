---
title: output/query
tags: [meta/tag-definition]
tag: output/query
kind: output
layers: [outputs]
frontmatter:
  required:    [title, tags, date]
  recommended: [description]
  optional:    []
template: output
---

# output/query

An answer to a question asked of the vault. Written by `query`, dated, and
partitioned to `outputs/query/` with a default retention of 365 days.

Every note it cites is wikilinked in the body — never listed in frontmatter —
so the answer appears in each cited note's backlink pane. States plainly when
the vault does not contain the answer rather than filling the gap from model
priors; a `**Gap:**` line naming what is missing is the expected shape for
that case.
