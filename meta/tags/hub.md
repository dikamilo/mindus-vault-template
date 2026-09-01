---
title: content/hub
tags: [meta/tag-definition]
tag: content/hub
kind: content
layers: [knowledge]
frontmatter:
  required:    [title, tags, hubs]
  recommended: [description]
  optional:    []
template: hub
---

# content/hub

The map for one directory in a `hub-tree` layer. Exactly one per folder, named after it, tagged with that level's hub tag — `content/hub` unless a layer's `structure.levels[]` names something more specific for that depth.

A hub note does not list its members; those are derivable from the filesystem, the `hubs` frontmatter field, and backlinks. Instead it carries orientation (what this domain is), a reading path (where to start and why), any open tensions, and links to sub-hubs and to the handful of notes that carry real weight. If its prose starts turning into a bulleted inventory, the hub should be split — see `system/conventions/structure.md`.

A hub note's own `hubs` field is the exception to "first entry matches your folder": it points at the hub of the folder *above* it, since the note itself is the folder's hub. A layer's root index carries no `hubs` at all.
