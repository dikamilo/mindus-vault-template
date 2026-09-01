# Authoring a lifecycle

Core has no notion of things that end — no `status/*`, no `flag/*`. A layer
that needs one declares its own `lifecycle:` block. This is distinct from
**note status** (a draft becoming stable), which is the `note-status`
vocabulary preset — lifecycle is about an *item* (a project, say) reaching a
terminal state; note status is about how finished the *writing* is.

## Format

```yaml
lifecycle:
  field: status
  values: [active, paused, done, archived]
  initial: active
  frontmatter: {required: [started], optional: [goal, ended]}
  on_archive: {action: move, to: <layer>/archive/, approval: required}
  stale_after: 90d
```

| Field | Values | Default |
|---|---|---|
| `field` | frontmatter field name | `status` |
| `values` | list | required |
| `initial` | one of `values` | first |
| `frontmatter.required` / `.optional` | field names the layer's hub notes carry | `[]` / `[]` |
| `on_archive.action` | `move` \| `tag-only` | `tag-only` |
| `on_archive.to` | path inside the same layer | required if `move` |
| `on_archive.approval` | `required` \| `none` | `required` |
| `stale_after` | duration | `never` |

## Notes on `on_archive.to`

An archive destination is an ordinary folder in the same layer — it gets a hub
note and an index entry exactly like any other top-level hub, created the same
way (`scaffold`, on approval). There is no privileged, exempt "archive" folder;
naming it with a leading underscore does not make it one. If a layer wants
archived items entirely out of sight, that is a case for moving them to a
*separate* layer, not for a special folder inside this one.

## Checks this turns on

Attaching a `lifecycle:` block to a layer makes `lifecycle` checks meaningful
there: `lifecycle-fields` (missing required frontmatter), `lifecycle-value` (a
value outside the enum), `lifecycle-stale` (still at `initial`, untouched past
`stale_after`). Add `lifecycle` to that layer's `checks:` — it is not implied
automatically by the presence of the block, the same way every other check
group is an explicit opt-in.
