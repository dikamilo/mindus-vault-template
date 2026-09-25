# The lint check registry

Every id usable in a layer's `checks:`. **M** = mechanical; **J** = judgement. Severity is the default and is overridable per layer with a map: `checks: {orphans: info}`.

## Group aliases

| Alias | Expands to |
|---|---|
| `core` | `frontmatter-parseable`, `frontmatter-required`, `content-tag-exactly-one`, `tag-known`, `tag-allowed-in-layer`, `title-present` |
| `frontmatter` | `frontmatter-parseable`, `frontmatter-required`, `title-present` — `core` without the `content/*` rules, for layers whose notes carry `output/*` instead |
| `structure` | `hub-per-folder`, `hub-naming`, `index-present`, `index-sync`, `index-lists-hubs-only`, `no-notes-at-root`, `level-hubs`, `depth-limit`, `hubs-field-present`, `hubs-primary-matches-folder`, `hubs-target-is-hub`, `hub-claim-reciprocal`, `hub-prose-is-list`, `level-folders`, `level-content` |
| `thresholds` | `hub-max-children`, `hub-underfull`, `note-max-words`, `note-min-words`, `promote-candidate`, `sibling-cluster` |
| `quality` | `duplicates`, `contradictions`, `orphans`, `atomicity`, `language`, `stale-note` |
| `links` | `links-resolve`, `embeds-resolve`, `links-inbound-policy`, `links-outbound-policy`, `links-isolated` |
| `flat` | `dated-filename`, `partition-valid`, `partition-tag`, `retention`, `age-window`, `output-links-notes`, `worklist-abandoned` |
| `lifecycle` | `lifecycle-fields`, `lifecycle-value`, `lifecycle-stale` |
| `all` | Everything applicable to the layer's archetype |

## Core

| Id | M/J | Severity | Catches |
|---|---|---|---|
| `frontmatter-parseable` | M | error | YAML that does not parse |
| `frontmatter-required` | M | error | A field in the layer's required list is missing |
| `frontmatter-recommended` | M | info | A recommended field is missing. In no alias, including `core` — recommended means optional, and a vault that has never filled in `description` should not open every lint run with a hundred of these. Name it explicitly on a layer where the recommendation matters |
| `content-tag-exactly-one` | M | error | Zero or two `content/*` tags where `require_exactly_one` |
| `tag-known` | M | warning | A tag with no definition in `meta/tags/`. Names the preset it came from where known |
| `tag-allowed-in-layer` | M | warning | A tag used in a layer its definition's `layers:` excludes |
| `title-present` | M | error | Missing `title` |

## Structure — `hub-tree` only

| Id | M/J | Severity | Catches |
|---|---|---|---|
| `hub-per-folder` | M | error | A folder with no hub note, or more than one |
| `hub-naming` | M | error | Hub filename ≠ folder name |
| `index-present` | M | error | `index: required` and no `index.md` at the root |
| `index-sync` | M | error | A top-level hub missing from the index, or an entry pointing at something gone |
| `index-lists-hubs-only` | M | warning | The index links a note rather than a hub |
| `no-notes-at-root` | M | error | A note directly at the layer root. Off at `depth: 0` |
| `level-hubs` | M | error | An enforced level missing its hub, or carrying the wrong tag |
| `depth-limit` | M | warning | Content deeper than `depth` where `beyond: strict` and depth is finite |
| `hubs-field-present` | M | error | A note with no `hubs`, outside the documented exemptions |
| `hubs-primary-matches-folder` | M | error | First `hubs` entry ≠ the note's folder hub, outside the documented exemptions — a hub note points at the folder *above* it, and a note below the enforced depth at the nearest hub above it |
| `hubs-target-is-hub` | M | error | A `hubs` entry pointing at a missing or non-hub note |
| `hub-claim-reciprocal` | J | warning | A secondary hub claim the claiming hub's prose never mentions |
| `hub-prose-is-list` | J | warning | Hub prose degenerating into an inventory |
| `level-folders` | M | error | At a level declaring `folders`: a folder whose name is not in the set, or a `required` folder missing under its parent hub. Silent where no level declares `folders` |
| `level-content` | M | error | A non-hub note whose `content/*` tag its folder does not accept — per `folders.<name>.content`, else `levels[].content`. Silent where neither is set |

## Thresholds

| Id | M/J | Severity | Reads |
|---|---|---|---|
| `hub-max-children` | M | warning | `thresholds.max_children` |
| `hub-underfull` | M | info | One child and no prose of its own. Skips `required` folders |
| `note-max-words` | M | warning | `thresholds.max_words` |
| `note-min-words` | M | info | `thresholds.min_words` if set |
| `promote-candidate` | M | info | `thresholds.promote_backlinks` |
| `sibling-cluster` | J | info | `thresholds.split_siblings` |

## Quality

| Id | M/J | Severity | Catches |
|---|---|---|---|
| `duplicates` | J | warning | Near-duplicate notes covering one concept — **across every `ingest_target` layer** |
| `contradictions` | J | info | Notes that disagree, across the same set |
| `orphans` | M | warning | No backlinks and unmentioned in any hub prose |
| `atomicity` | J | info | A note carrying two independent concepts |
| `language` | J | warning | Content not in the layer's `language`. In a `language: any` layer it still checks the **structural** notes — the layer index and every hub note — against the vault language |
| `stale-note` | M | info | Neither `updated` nor the last `Review: confirmed` record is within `policy.staleness_window` |
| `stale-flag` | M | info | **Only when `note-flags` is attached:** any `flag/*` older than the staleness window |

## Links and assets

| Id | M/J | Severity | Catches |
|---|---|---|---|
| `links-resolve` | M | warning | A wikilink with no target |
| `embeds-resolve` | M | warning | An embed with no target |
| `links-inbound-policy` | M | error | A link from a layer the target's `inbound_from` excludes. **Ignores links whose source is the `outputs` role** |
| `links-outbound-policy` | M | error | A link to a layer this layer's `outbound_to` excludes |
| `links-isolated` | M | error | A link between two different subtrees rooted at the level named in `links.isolate` — hub notes included. The layer `index.md` sits above every subtree and is exempt. **Ignores links whose source is the `outputs` role** |
| `orphan-assets` | M | info | A file in an `assets` layer no note references |

## Flat layers

| Id | M/J | Severity | Catches |
|---|---|---|---|
| `age-window` | M | warning | Files in a `consumable` layer older than `policy.ingest_window` |
| `dated-filename` | M | warning | A file in a `dated` layer without a date prefix |
| `partition-valid` | M | warning | A file outside every declared partition |
| `partition-tag` | M | warning | A file whose tag ≠ its partition's declared tag |
| `retention` | M | info | Files past their partition's `retention`, reported as prunable. **Never names a worklist with open findings** |
| `output-links-notes` | J | info | An output naming vault notes in prose without wikilinking them, breaking the backlink mechanism findings depend on |
| `worklist-abandoned` | M | info | Findings open beyond the staleness window — never triaged, or accepted and never acted on |

## Lifecycle — preset layers only

| Id | M/J | Severity | Catches |
|---|---|---|---|
| `lifecycle-fields` | M | error | Missing required lifecycle frontmatter |
| `lifecycle-value` | M | error | A value outside the declared enum |
| `lifecycle-stale` | M | info | An item still at `initial` and untouched beyond `stale_after` |

## Config — run by `configure`, not `lint`

`config-schema`, `config-paths-exist`, `config-refs-exist`, `config-no-overlap`, `config-link-policy-agrees`, `config-roles-unique`, `config-checks-known`, `config-unknown-skill`, `config-vocabulary-agrees`, `config-unused`.
