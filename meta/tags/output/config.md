---
title: output/config
tags: [meta/tag-definition]
tag: output/config
kind: output
layers: [outputs]
frontmatter:
  required:    [title, tags, date]
  recommended: [description]
  optional:    []
template: output
---

# output/config

A record of a configuration change, written by `configure` to
`outputs/configure/` and kept indefinitely. Every mutation to `meta/` writes one
of these — with a diff of what changed — before the mutation is applied, since
`configure` is the only skill that writes `meta/` and every change it makes is
approval-gated.
