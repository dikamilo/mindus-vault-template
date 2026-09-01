# `knowledge-layer` preset

Adds a second `ingest_target` layer alongside `knowledge`, for a domain that deserves its own scope and voice without living in the same hub tree. Reuses the core `content/concept` tag rather than shipping a new one.

## What installing this actually touches

Beyond the usual layer install (folder, `vault.yaml` block, `index.md` via `scaffold`):

- `content/concept`'s `layers:` field gains this layer's name.
- Every *existing* `ingest_target` layer's `links.inbound_from` and `outbound_to` gain this layer's name, and this layer's own lists are seeded with theirs — both sides, in the same operation, or `config-link-policy-agrees` fails.
- Turning `ingest_target` on brings this layer into scope for cross-layer `duplicates` and `contradictions` checking, alongside every other ingest-target layer.

## When you want this

Two genuinely separate domains you ingest into, wanting different thresholds, voice, or vocabulary between them — e.g. technical knowledge with a terse voice, and reflective/soft-skills material with a different one. If one voice and one vocabulary would serve both, you don't need a second layer; add hubs to the one you have instead.
