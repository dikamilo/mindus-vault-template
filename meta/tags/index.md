---
title: content/index
tags: [meta/tag-definition]
tag: content/index
kind: content
layers: [knowledge]
frontmatter:
  required:    [title, tags]
  recommended: [description]
  optional:    []
template: index
---

# content/index

The entry point to a `hub-tree` layer: one per layer root, carrying no `hubs` field because it has nothing above it.

An index links to the top-level hubs of its layer, and only those — the doors into the layer, each with one line saying what is behind it. It does not descend: a top-level hub appears, its children do not. It is the one place the vault stores a list rather than computing one, because the enumeration a directory listing gives you carries no sense of what matters or where to start.

Every layer with `structure.index: required` needs exactly one, maintained by whichever skill added, renamed or removed a top-level hub — in the same operation, never as a later cleanup.
