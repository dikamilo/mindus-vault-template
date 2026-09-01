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

The hub tag for the `projects` layer's one enforced level: one project, one hub, named after its folder like any other hub, plus the lifecycle fields this preset's `lifecycle:` block requires (`status`, `started`).

Its prose is the project's own reading path — what it is, what state it's in, and what the [[decision]] notes underneath it were actually about. It does not list them; that is what the folder and backlinks are for.
