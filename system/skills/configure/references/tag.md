# Authoring a tag

One file per tag under `meta/tags/` (or, for a preset's own tags, under its
`tags/` folder before install). The frontmatter is the machine-checkable part;
the body is the prose that helps an agent choose between two similar tags.

## Format

```markdown
---
title: <namespace>/<value>
tags: [meta/tag-definition]
tag: <namespace>/<value>
kind: content | output | meta
layers: [<layer names this tag may appear in>]
frontmatter:
  required:    [title, tags, hubs]
  recommended: [description, created, updated]
  optional:    [aliases]
length: {min: 100, max: 500, unit: words}   # content tags only, if it applies
template: <template name>
---

# <namespace>/<value>

<one paragraph: what this type is for, and what would make a note choose it
over its nearest neighbour.>
```

## Rules

- **Namespace matters.** `content/*` is what a note *is* (exactly one per
  note, unless the layer sets `require_exactly_one: false`); `output/*` is a
  kind of generated artifact, valid only in the outputs layer; `meta/*`
  describes files under `meta/` itself. Topic categorization is never a tag
  namespace — that is what hubs are for, and a topic tag would create a second
  source of truth alongside the hierarchy.
- **`layers:` is the other half of `vocabulary.content` / `vocabulary.tags`.**
  A layer says which tags it accepts; a tag says which layers it may appear
  in. Both directions must agree — `config-vocabulary-agrees` checks this —
  which is why attaching a preset updates both sides in the same operation.
- A missing tag definition is a `tag-known` warning, not an error (P5) — a
  vault never refuses a note for an undefined tag, but write the definition
  anyway so the next person (or agent) knows what the tag means.

## Adding a tag to an existing layer

1. Write (or copy) the tag file into `meta/tags/`.
2. Add the bare value to that layer's `vocabulary.content` (or the full tag to
   `vocabulary.tags`, for a non-`content/*` tag) in `vault.yaml`.
3. Add the layer's name to the tag's `layers:` field.
4. If the tag implies frontmatter (e.g. a lifecycle field), add it to the
   layer's `vocabulary.frontmatter.recommended`.
5. Validate and log, as with any other configuration change.
