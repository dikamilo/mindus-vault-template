---
title: content/idea
tags: [meta/tag-definition]
tag: content/idea
kind: content
layers: []
frontmatter:
  required:    [title, tags, hubs]
  recommended: [description, created]
  optional:    []
length: {min: 30, max: 300, unit: words}
template: idea
---

# content/idea

Something the project *might* do — not yet chosen. An idea note is a lightweight proposal: the idea itself, why it might matter, and the open questions that would have to be answered before acting on it. It lives in its project's `ideas/` folder, and its primary `hubs` entry is that folder's `ideas` hub in path form — `[[<layer>/<project>/ideas/ideas|ideas]]` — because every project has an `ideas` hub and a bare `[[ideas]]` is ambiguous.

Choose it over [[decision]] when nothing has been settled yet. An idea carries no status: when one is acted on (or explicitly rejected), write a `content/decision` note recording that choice and link it back to the idea, rather than editing the idea into a record of what happened.
