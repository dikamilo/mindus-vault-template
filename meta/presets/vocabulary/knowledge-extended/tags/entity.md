---
title: content/entity
tags: [meta/tag-definition]
tag: content/entity
kind: content
layers: []
frontmatter:
  required:    [title, tags, hubs]
  recommended: [description, created, updated]
  optional:    [aliases]
length: {min: 50, max: 400, unit: words}
template: entity
---

# content/entity

A specific, named thing — a person, organisation, tool, product, or place —
rather than an idea about it. If a note is describing *what X is and does* as a
proper noun with a fixed identity, it wants this tag; if it's explaining a
mechanism or principle, it wants `content/concept` instead.
