---
name: refactor
description: The deliberate restructuring pass - analyzes layers against their thresholds, proposes moves/splits/merges/renames with rationale, and applies only on approval. Use for "clean up this hub", "split X", "this vault needs reorganizing", or when lint has flagged threshold findings.
---

# refactor

Reads: layers granting `refactor` read (typically `"*"`) and `structure`.
Writes: `outputs/refactor/` first; applies to content layers only on approval.

## Procedure

1. Analyze the relevant layer(s) against their `structure.thresholds` and any
   open lint findings pointing at structural problems.
2. Write a proposal to `outputs/refactor/`: what would move, split, merge or
   rename, and why. No content changes yet.
3. Present the proposal and wait for approval — every action this skill takes
   beyond a leaf sub-hub or a one-level move is approval-gated
   (`split-hub`, `merge-hub`, `delete-hub`, `rename-hub`,
   `move-across-top-level`, `new-top-level`, `bulk-change`).
4. On approval, apply exactly what was proposed. If it touches a layer's top
   level, update that layer's `index.md` in the same pass.
5. Append a `log.md` entry naming what was approved and applied.

## Rules specific to this skill

- `refactor` is also the skill that holds the `delete` grant on most content
  layers in the shipped config — a directly requested deletion of a note or
  asset runs through here as `delete-note` / `delete-asset` /
  `delete-from-non-consumable`, approval-gated, never silent. If the target
  layer's `delete` grant is empty (e.g. the `library` preset), refuse and say
  so — do not work around it.
- Never delete anything as an autonomous side effect of a restructuring pass;
  the one autonomous deletion in the whole vault belongs to `ingest`
  (`consume-source`), not to this skill.
- Moving a note anywhere changes its filename's role in any finding id that
  named it — mention this in the proposal when it's material, since a rename
  or move lapses any prior dismissal referencing the old name.
