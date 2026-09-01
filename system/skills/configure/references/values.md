# Configuration value reference

## Scalar formats

| Format | Grammar | Examples |
|---|---|---|
| Duration | `<n>d` \| `<n>w` \| `<n>m` \| `<n>y` \| `keep` \| `never` | `30d`, `12w`, `6m`, `keep` |
| Language | ISO 639-1 code, or `inherit` \| `any` | `en`, `pl`, `inherit` |
| Skill list | List of skill names, or `"*"`, or `[]` | `[ingest, capture]`, `"*"` |
| Path | Vault-relative, trailing slash for folders | `knowledge/` |
| Tag | Namespace/value | `content/concept` |

## Top-level keys

| Key | Values |
|---|---|
| `mindus.spec_version` | integer. The schema this config was written against |
| `mindus.name` | string. The vault's name; its template default is what marks a vault unpersonalized |
| `mindus.language` | ISO 639-1 code. The vault language every layer inherits |
| `excluded` | list of paths lint and query ignore entirely. `[meta/, system/]` |
| `policy` | see below |
| `voice` | see below |
| `defaults` | a partial layer block merged under every layer before its own values |
| `templates` | map of template name → `{file, for, variables}`. `for` is a tag or tag glob; `variables` is the list of `{{names}}` the skeleton uses |
| `layers` | map of layer name → layer block |

## Layer fields

| Field | Values | Default |
|---|---|---|
| `path` | path | required |
| `archetype` | `hub-tree` \| `flat` | `hub-tree` |
| `title` | string, shown in generated renderings | derived from the layer name |
| `description` | one sentence, shown in renderings and offered to `query` for orientation | none |
| `templates` | map of slot → template name. Slots: `index`, `hub`, `default` | layer type defaults |
| `role` | `inbox` \| `outputs` \| `assets` | none |
| `protected` | bool | `false` |
| `holds` | `notes` \| `files` | `notes` |
| `language` | `inherit` \| `any` \| code | `inherit` |
| `discoverable` | bool | `true` |
| `ingest_target` | bool | `false` |
| `scope` | prose | none |
| `voice` | map of context → voice name; a bare string means `{notes: <name>}` | resolved per below |
| `doc` | path to a markdown note | none |
| `checks` | list of ids/aliases, or a map of id → severity | `[core]` |

## `structure` — `hub-tree`

| Field | Values | Default |
|---|---|---|
| `depth` | non-negative integer \| `unbounded`. `0` = index only | `unbounded` |
| `beyond` | `strict` \| `free` | `strict` |
| `index` | `required` \| `none` | `required` |
| `levels[].tag` | a `content/*` tag | `content/hub` |
| `levels[].name` | label used in messages | none |
| `levels[].template` | template name | layer's `templates.hub` |
| `thresholds.max_children` | integer \| `off` | `15` |
| `thresholds.split_siblings` | integer \| `off` | `3` |
| `thresholds.promote_backlinks` | integer \| `off` | `8` |
| `thresholds.max_words` | integer \| `off` | `800` |
| `thresholds.min_words` | integer \| `off` | `off` |

## `flat`

| Field | Values | Default |
|---|---|---|
| `consumable` | bool | `false` |
| `dated` | bool | `false` |
| `retention` | duration | `keep` |
| `partitions.by` | `writer` \| `list` \| `none` | `none` |
| `partitions.defaults.retention` | duration | `keep` |
| `partitions.defaults.group_by` | `none` \| `month` \| `year` | `none` |
| `partitions.overrides.<name>.tag` | tag | none |
| `partitions.overrides.<name>.retention` | duration | `partitions.defaults.retention` |
| `partitions.overrides.<name>.group_by` | `none` \| `month` \| `year` | `partitions.defaults.group_by` |
| `partitions.overrides.<name>.routes_to` | layer name | none |

With `by: writer`, each partition is named for the skill that writes it and its
key here is that skill's name. With `by: list`, the keys are the folder names.

## `vocabulary`, `links`, `access`

| Field | Values | Default |
|---|---|---|
| `vocabulary.content` | list of `content/*` values, bare names. Covers **notes below hub level only** — a layer's hub and index tags come from `structure.levels[].tag` and `content/index`, and never need listing here | `[]` |
| `vocabulary.require_exactly_one` | bool | `true` |
| `vocabulary.tags` | additional non-`content/` tags this layer accepts, e.g. `status/*` from an attached vocabulary preset | `[]` |
| `vocabulary.frontmatter.required` / `.recommended` | field names | `[title, tags, hubs]` / `[]` |
| `links.inbound_from` | list of layer names, `"*"`, `[]` | `"*"` |
| `links.outbound_to` | same | `"*"` |
| `access.read` / `.write` / `.structure` / `.delete` | skill list | `"*"` / `[]` / `[]` / `[]` |

## `policy` and action ids

| Field | Values | Default |
|---|---|---|
| `ingest_window` | duration | `30d` |
| `staleness_window` | duration | `180d` |
| `log_rollover` | `none` \| `yearly` | `yearly` |
| `default_ingest_target` | layer name | first `ingest_target` layer |
| `bulk_threshold` | integer | `10` |
| `autonomy.autonomous` / `.approval` / `.never` | action ids | see `meta/vault.yaml` |

Every action id must appear in exactly one class.

| Id | Action |
|---|---|
| `create-note` | Create a note in an existing hub |
| `extend-note` | Add to an existing note |
| `create-leaf-hub` | Create a sub-hub at the leaf |
| `move-one-level` | Move a note into a sub-hub of its parent |
| `cross-link` | Add links and secondary `hubs` entries |
| `rewrite-hub-prose` | Rewrite a hub's orientation prose |
| `save-asset` | Write a file into the assets layer |
| `consume-source` | Delete a source from a `consumable: true` layer after a successful ingest |
| `update-index` | Update a layer `index.md` |
| `append-log` | Append a prose entry to the log |
| `record-decision` | Append a `Triage:` or `Review:` record |
| `split-hub` | Split a hub with many children |
| `merge-hub` | Merge two hubs |
| `delete-hub` | Delete a hub |
| `rename-hub` | Rename a hub |
| `move-across-top-level` | Move a note between top-level domains |
| `new-top-level` | Introduce a new top-level hub |
| `delete-note` | Delete a note |
| `delete-asset` | Delete a file from the assets layer |
| `delete-from-non-consumable` | Delete from a layer that is not a queue |
| `bulk-change` | Any change touching more than `bulk_threshold` files |
| `install-preset` | Install a preset |
| `attach-vocabulary` | Attach a vocabulary preset to a layer |
| `edit-config` | Modify `meta/` |
| `edit-system` | Modify `system/` |

## `voice`

| Field | Values |
|---|---|
| `voice.default` | path to a markdown file. Required |
| `voice.contexts.<name>` | path to a markdown file. Shipped contexts: `notes`, `hubs`, `query`, `synthesis`. Presets and `configure` may register more |
| `layer.voice` | map of context name → voice name, overriding that context inside this layer. A bare string is shorthand for `{notes: <name>}` |

Contexts name *what is being written*, not who writes it. Resolution for a
given piece of writing, most specific first:
`layer.voice.<context>` → `voice.contexts.<context>` → `voice.default`.

## `lifecycle` — preset layers

| Field | Values | Default |
|---|---|---|
| `field` | frontmatter field name | `status` |
| `values` | list | required |
| `initial` | one of `values` | first |
| `frontmatter.required` / `.optional` | field names the layer's hub notes carry | `[]` / `[]` |
| `on_archive.action` | `move` \| `tag-only` | `tag-only` |
| `on_archive.to` | path inside the same layer. It is an ordinary folder there, so it gets a hub note and an index entry like any other — do not name it with a leading underscore expecting it to be exempt | required if `move` |
| `on_archive.approval` | `required` \| `none` | `required` |
| `stale_after` | duration | `never` |

## `preset.yaml`

Not part of `vault.yaml`, but validated by `configure` and therefore part of
the same reference.

| Field | Values |
|---|---|
| `preset.name` | string, matching the directory name |
| `preset.version` | integer. A label; nothing acts on it |
| `preset.spec_version` | integer. `configure` warns when it is older than the vault's |
| `preset.description` | one sentence, shown when offering presets |
| `preset.provides.tags` | tags this preset installs into `meta/tags/` |
| `preset.provides.templates` | template names it installs and registers |
| `preset.provides.voice` | voice context names it installs and registers |
| `preset.requires` | other preset names, installed and attached first |
| `preset.prompts[]` | `{key, question, default}`. Answers substitute `{{key}}` in the layer block |
| `layer` | a full layer block. **Optional** — a preset without one is a vocabulary preset |
