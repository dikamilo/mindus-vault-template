# `projects` preset

A shallow hub-tree: one folder per project, its hub carrying `status` and `started`, and underneath it exactly two sub-hubs — `decisions/` for `content/decision` notes and `ideas/` for `content/idea` notes. Archiving moves the project's folder to `<layer>/archive/` on approval — an ordinary folder there, not a hidden bin.

```
projects/
  index.md
  <project>/
    <project>.md          content/project
    decisions/
      decisions.md        content/hub, hubs: [[<project>]]
      <decision>.md       content/decision
    ideas/
      ideas.md            content/hub, hubs: [[<project>]]
      <idea>.md           content/idea
```

## When you want this

You have discrete efforts with a beginning and an end — as opposed to `areas`, which are ongoing responsibilities that never finish. If something never reaches "done", it belongs in `areas` instead, or in `knowledge` if it isn't really yours to run.

## What belongs here versus elsewhere

- A decision made *while working on* the project → `content/decision` in that project's `decisions/`.
- Something the project *might* do, not yet settled → `content/idea` in that project's `ideas/`. Once acted on or rejected, record the choice as a decision that links back to the idea.
- A concept you learned *because of* the project, useful beyond it → the `knowledge` layer instead. Cross-link it from the project hub if the connection is worth surfacing.
- A recurring responsibility with no project shape → `areas`.

## Declared shape

The layout above is config, not convention: `structure.levels` declares that a project folder holds no notes of its own and exactly the two `required` sub-folders, each accepting one tag, and `links.isolate: project` forbids links between projects. Lint checks all of it (`level-folders`, `level-content`, `links-isolated`); `scaffold` creates both sub-hubs with every project.

Two rules remain conventions:

- **Sub-hubs link up, not down.** `decisions.md` and `ideas.md` carry `hubs: [[<project>]]`; the project note does not link to them — a deliberate exception to the vault's "hub prose links its sub-hubs" guidance, since every project has the same two and backlinks already show them.
- **Path-form links to sub-hubs.** Every project has a `decisions.md` and an `ideas.md`, so a bare `[[decisions]]` is ambiguous. Link sub-hubs — including in `hubs` frontmatter — as `[[<layer>/<project>/decisions/decisions|decisions]]`.
