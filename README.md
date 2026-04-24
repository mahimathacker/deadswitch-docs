# DeadSwitch Docs

Documentation site for **DeadSwitch** — an on-chain crypto inheritance protocol.

Built with [MkDocs](https://www.mkdocs.org/) using the Material theme and deployed to GitHub Pages at <https://mahimathacker.github.io/deadswitch-docs>.

## Local development

```bash
pip install mkdocs-material mkdocs-minify-plugin
mkdocs serve
```

Then open <http://127.0.0.1:8000>.

## Build

```bash
mkdocs build
```

## Structure

- `mkdocs.yml` — site configuration and navigation
- `docs/` — Markdown source files
  - `index.md` — home
  - `how-it-works.md`
  - `getting-started.md`
  - `security.md`
  - `gas-optimizations.md`
- `.github/workflows/` — GitHub Pages deploy workflow

## Deployment

Pushes to `master` trigger the GitHub Actions workflow that publishes the site to GitHub Pages.
