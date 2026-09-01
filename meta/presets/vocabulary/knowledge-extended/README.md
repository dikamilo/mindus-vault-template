# `knowledge-extended` vocabulary preset

Three additional `content/*` types — `entity`, `method`, `fact` — for material that `content/concept` doesn't fit well. Attach it to any layer that wants finer-grained content types than the bare `knowledge` default.

## When you want this

`content/concept` alone starts feeling like it's covering three different jobs: explanations of mechanisms, step-by-step procedures, and short standalone claims. If a layer's notes are drifting that way, attach this preset and let `ingest`/`capture` pick the closest type per note going forward — it does not retag anything that already exists.
