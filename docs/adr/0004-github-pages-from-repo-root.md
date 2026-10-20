# ADR-0004: Published by GitHub Pages from the repository root

**Decision**
The site is served by GitHub Pages from branch `main`, folder `/ (root)`. `index.html` sits at
the repository root — not in a `site/` subfolder — and `.nojekyll` sits beside it. The
repository is public.

**Reason**
Pages on a free account requires a public repository. Serving from the root is what makes
`https://<user>.github.io/<repo>/` resolve to the app with no path juggling. `.nojekyll` turns
off Jekyll preprocessing, which the page does not need and which can drop files.

**Consequences**
- Everything committed here is served publicly. `.ai/`, `docs/` and `CLAUDE.md` are reachable
  over HTTP; that is accepted — they are not linked and the repo is public anyway.
- Moving `index.html` into a subfolder breaks the site unless the Pages source moves with it.
- A 404 after a push usually means Pages is pointed at the wrong branch or folder, or
  `index.html` is not at the root — not that the build failed.
- **`.nojekyll` is currently missing from this repository.** A 0-byte file named `download` sits
  at the root in its place, almost certainly the same file mangled during upload. Add
  `.nojekyll`, delete `download`.
