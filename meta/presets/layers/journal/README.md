# `journal` preset

A flat, dated pile of personal entries — no hub grammar, no index, entries are
peers identified by date. The only shipped preset that is `flat` and holds
notes a human writes directly rather than material a skill manages, which is
what makes it worth building and testing early (design spec §11.4): it is the
archetype's one real exercise outside the machine-managed layers.

## When you want this

Free-form, dated personal writing that isn't meant to be distilled or filed
under a hub — reflections, daily notes, anything you'd write in a paper
journal. If it contains a concept worth keeping and reusing, capture that
separately into `knowledge` (or `areas`); the journal entry itself stays as the
record of when and how you thought it.

## Filenames

Date-prefixed: `2026-08-19-<slug>.md`. `dated-filename` lint checks for this.
