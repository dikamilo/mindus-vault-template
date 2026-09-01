# Authoring, installing and exporting a preset

A preset is a bundle of vault configuration living under `meta/presets/`, shipped populated and installed on request — presence in the catalog is not installation. There are two kinds, and the difference decides which folder a preset goes in.

| | Layer preset (`presets/layers/`) | Vocabulary preset (`presets/vocabulary/`) |
|---|---|---|
| Answers | "What new region of the vault do I want?" | "What kinds of note may live in a region I already have?" |
| Contains | `layer:` block + tags + templates + voice | Tags, templates, voice — no `layer:` |
| Installing | Appends a layer to `vault.yaml`, creates the folder, hands off to `scaffold` for the index | Copies artifacts into `meta/`, then **attaches** |
| Reusable | No — each install is a new layer | Yes — one install, any number of attachments |
| Uninstalling | Refuses while the folder has content | Detaches everywhere, keeps files on notes, warns |

## Format

```
meta/presets/layers/<name>/
├── preset.yaml
├── tags/{<tag>.md, …}
├── templates/{<template>.md, …}
├── voice/<name>.md          # optional
└── README.md
```

```yaml
preset:
  name: <name>
  version: 1
  spec_version: 3
  description: <one sentence>
  provides:
    tags:      [<content/* values this preset installs>]
    templates: [<template names>]
    voice:     [<voice context names, if any>]
  requires: []                # other preset names, installed+attached first
  prompts:
    - {key: path, question: "Folder name?", default: <name>/}

layer:                        # omit entirely for a vocabulary preset
  path: "{{path}}"
  # … the full layer block, see references/layer.md
```

`README.md` carries the judgement the config cannot: *when* you would want this layer or vocabulary, and what belongs in it versus somewhere else.

## Validation before install

Schema-valid `preset.yaml`; every tag in `provides.tags` present under `tags/`; every template the layer block references present under `templates/`; every `checks:` id in the registry; no reference to a layer outside itself.

## Install vs. attach

Installing copies files into `meta/` and registers them — idempotent, touches no layer. For a layer preset, it additionally appends the `layer:` block, creates the folder, and calls `scaffold` for the index note. Attaching (a vocabulary preset onto a layer) is the separate, repeatable step that actually puts a tag in scope for a specific layer — see `references/tag.md` for what it writes on both sides. Attaching something that makes no sense (a note-shaped vocabulary onto a `holds: files` layer) warns rather than refusing.

## Uninstalling

Never rewrite notes. Count how many carry the preset's tags, warn that they will show as unknown-tag lint findings once detached, and proceed only on confirmation. No exemption list — that would be a second source of truth about which tags are valid.

## Exporting a layer you configured by hand

The install path run backwards: write `meta/presets/layers/<name>/preset.yaml` with the layer block parameterised (whatever varies goes behind a `prompts[]` entry, most commonly `path`), and copy every tag, template and voice file the layer's config currently references into the preset folder.
