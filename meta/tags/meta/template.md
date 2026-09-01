---
title: meta/template
tags: [meta/tag-definition]
tag: meta/template
kind: meta
layers: []
---

# meta/template

Marks a file under `meta/templates/` as a note skeleton. A template's contract —
what it is for, which tag it applies to, what `{{variables}}` it takes — lives
in `vault.yaml`'s `templates:` map, not in a second frontmatter block on the
template file itself; templates are rendered by the agent, not by a plugin, so
they stay plain text with `{{name}}` placeholders.
