# econpotter.github.io

Quarto site. `_quarto.yml` renders `index.qmd` -> `docs/` (GitHub Pages source).
Rendering is NOT automatic — `.github/workflows/pages.yml` only publishes
whatever is already committed in `docs/`; it does not run `quarto render`.

## Rendering on commit

A git hook re-renders the site whenever a staged `.qmd` file changes:

```
git config core.hooksPath .githooks
```

Run this once per clone (hooksPath is a local git config, not carried by
`git clone`). Requires `quarto` on PATH. The hook (`.githooks/pre-commit`)
runs `quarto render` and stages the resulting `docs/` changes automatically.
