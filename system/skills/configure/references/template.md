# Authoring a template

A template is a pure skeleton under `meta/templates/` — no frontmatter of its
own describing itself. Its contract lives in `vault.yaml`'s `templates:` map:

```yaml
templates:
  <name>: {file: meta/templates/<name>.md, for: <tag or glob>, variables: [<names>]}
```

- `for` is the tag (or tag glob, e.g. `"output/*"`) the template applies to.
- `variables` lists every `{{name}}` placeholder the skeleton uses. A skill
  rendering the template supplies exactly these.
- Placeholders are `{{name}}`, rendered by the agent, not by a plugin.
  **`meta/templates/` must never be set as Obsidian's own template folder** —
  `{{date}}` and `{{title}}` collide with the core Templates plugin's syntax.

## Writing one

Keep it to frontmatter plus a single `# {{title}}` heading and, at most, a
one-line body prompt (see the shipped templates for the shape). A template is
not the place for guidance on what to write — that belongs in the tag
definition's prose (`references/tag.md`) or the relevant voice file.

## Registering a new one

1. Write the skeleton file.
2. Add its entry to `templates:` in `vault.yaml`.
3. Reference it from a layer's `templates:` map (slots: `index`, `hub`,
   `default`) or a `structure.levels[].template`, wherever it should be used.
4. Validate: the file exists, `for` names a real tag or glob, no `{{variable}}`
   in the file is missing from the registered list or vice versa.
