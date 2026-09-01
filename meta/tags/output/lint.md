---
title: output/lint
tags: [meta/tag-definition]
tag: output/lint
kind: output
layers: [outputs]
frontmatter:
  required:    [title, tags, date]
  recommended: [description]
  optional:    []
template: output
---

# output/lint

A dated health-check worklist, written by `lint` to `outputs/lint/`, grouped by month, retained 90 days — except that **a report with any open finding is never named as prunable**, no matter its age. See `## Findings` as markdown checkboxes; a report is closed only once every box is ticked, and closed-ness is derived from the boxes, never stored separately.

Every note or finding it names is wikilinked, so the report shows up in the backlink pane of everything it flags. `review` reads these back to know what has already been triaged.
