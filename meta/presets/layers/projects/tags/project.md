---
title: content/project
tags: [meta/tag-definition]
tag: content/project
kind: content
layers: []
frontmatter:
  required:    [title, tags, hubs, status, started]
  recommended: [description, goal]
  optional:    [ended]
template: project
---

# content/project

The hub tag for the `projects` layer's first enforced level: one project, one hub, named after its folder like any other hub, plus the lifecycle fields this preset's `lifecycle:` block requires (`status`, `started`).

Its shape is declared in the layer's `structure.levels`: every project holds exactly a `decisions/` and an `ideas/` sub-hub, created with it, and nothing else. Each sub-hub's `hubs` field points at its project hub, `[[<project>]]`; that field and its backlink are the whole connection — the project note does not link to its sub-hubs in its prose. Their names repeat in every project, so any link to one — including a note's `hubs` entry — uses the path form, `[[<layer>/<project>/decisions/decisions|decisions]]`, never the bare name.

Its prose is the project's own orientation — what it is, what state it's in, what it is aiming at. It neither lists nor links its decisions and ideas; that is what the folders, the `hubs` fields and backlinks are for.
