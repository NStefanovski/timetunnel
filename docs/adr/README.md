# Architecture decision records

Short, numbered notes on decisions a future session would otherwise rediscover or reopen.
One decision per file, `NNNN-kebab-title.md`, ~15–30 lines.

Superseding: leave the old file, add `**Superseded by ADR-00NN**` at its top, write the new one.

| # | Decision |
|---|---|
| [0001](0001-published-artifact-leads-the-generator.md) | The published `index.html` leads its own generator — do not rebuild |
| [0002](0002-no-framework-no-build-step.md) | No framework, bundler, or runtime dependency |
| [0003](0003-localstorage-only-persistence.md) | Progress lives in `localStorage`; there is no backend |
| [0004](0004-github-pages-from-repo-root.md) | Published by GitHub Pages from the repository root |
