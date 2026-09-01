---
title: meta/tag-definition
tags: [meta/tag-definition]
tag: meta/tag-definition
kind: meta
layers: []
---

# meta/tag-definition

Marks a file under `meta/tags/` as the definition of one tag. `meta/` is not a
layer — this tag exists so the files there are self-describing, not so a skill
searches for it; `configure` reads and writes these files directly by path.

A definition's frontmatter is the machine-checkable part (`tag`, `kind`,
`layers`, `frontmatter`, `template`, and — for content tags — `length`); its
body is the prose that helps an agent choose between two tags when a note could
plausibly carry either.
