---
title: content/concept
tags: [meta/tag-definition]
tag: content/concept
kind: content
layers: [knowledge]
frontmatter:
  required:    [title, tags, hubs]
  recommended: [description, created, updated]
  optional:    [aliases]
length: {min: 100, max: 500, unit: words}
template: concept
---

# content/concept

An idea, mechanism or principle that can be understood on its own.

A concept note answers "what is this and why does it work that way" — a mechanism, an idea, a principle. If a note is drifting into "how do I do this", or into a running record of one tool or person, it wants a narrower type than this one; check the layer's vocabulary for what is available. `knowledge` ships with only this type — presets such as `knowledge-extended` add `entity`, `method` and `fact` for the cases this one does not fit.

One note, one concept: past the length guidance above, or once it acquires two independent sections pointing at different neighbours, it is a split candidate.
