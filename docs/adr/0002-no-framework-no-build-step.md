# ADR-0002: No framework, bundler, or runtime dependency

**Decision**
The page is vanilla HTML, CSS and JavaScript in one file. There is no React, no bundler, no
`package.json`, no npm dependency. The only external resource is Google Fonts. The illustrations
are drawn with a hand-rolled canvas engine (`paint`, `stroke`, `poly`, `ell`, `glow`) rather
than a drawing library.

**Do not** add a framework, bundler, or npm dependency without asking first.

**Reason**
The deliverable is a single file anyone can open from disk with no toolchain. Every dependency
added is a build step, an install step, and a way for that to stop being true.

**Consequences**
- Run it by opening `index.html`, or `python3 -m http.server 8000` to avoid `file://` quirks.
- New illustration work matches the existing primitives rather than introducing a library.
- There is no test suite and no linter; changes are verified by loading the page and exercising
  the three views (time machine, scroll, quiz).
- Any dependency would have to be inlined by `build.py` to preserve the single-file property.
