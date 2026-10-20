# Time Tunnel — Agent Instructions

An interactive timeline of human history and world myths. Swipe the timeline and a
lantern-bearer walks through hand-painted canvas scenes, one per story. Vanilla HTML/CSS/JS,
no framework, no bundler, no dependencies beyond Google Fonts.

## Read this first

**`index.html` is the finished, published artifact and it is ahead of its own generator.**
Line 7 says *"Built by build.py from stories.json. Edit the stories there, not here."* — but the
animated diorama (the lantern-bearer and the 262 painted scenes) was added to the output
**after** that, and `build.py`'s template does not produce it.

So the danger runs the opposite way to the usual one:

- **Do not hand-edit `index.html`** — a later build would discard the change.
- **Do not run `build.py` either**, until its template carries the diorama. Regenerating right
  now silently drops the entire diorama from the page. The three pieces the template needs
  (`diorama.css`, `diorama-canvas.html`, `diorama-script.html`) ship in the handoff package,
  not in this repository. See `docs/adr/0001-published-artifact-leads-the-generator.md`.
- Neither `build.py` nor `stories.json` is in this repository. A content change has no safe
  route here today — say so rather than editing the artifact.

**This repo is the GitHub Pages site.** `index.html` must stay at the repo root, the repo must
stay public, and `.nojekyll` must exist at the root. Everything committed here is served
publicly.

**`index.html` is 510 KB / 3,339 lines, and line 359 alone is ~300 KB** (the whole content
corpus as one JSON line). Never open the file whole — roughly 130k tokens, most of a context
window. Never `grep` it untruncated either; one match on line 359 dumps 300 KB.

```bash
grep -n "pattern" index.html | cut -c1-110     # always pipe through cut
sed -n '649,812p' index.html                   # read a section by line range
```

`.ai/repo-map.md` has the line-range index. Go there first, then read the range you need.

## Context policy

1. Start with `.ai/repo-map.md`; never scan the repo to get oriented.
2. Search before opening, and truncate every search (`| cut -c1-110`).
3. Read line ranges, not files. Never re-read a range already in context.
4. Use git as memory: `git log --oneline -10`, `git diff`, `git show <sha>`.
5. Review your own work from `git diff`, not by re-reading.
6. Record durable decisions in `docs/adr/`; never re-litigate one already written down.
7. One conversation = one task. Checkpoint to `.ai/tasks/<task>.md` and start fresh rather
   than growing a long thread.
8. Aim for minimum *irrelevant* context, not minimum context.

## Git

Ask before `commit`, `push`, `branch`, `rebase`, `merge`, or any `reset` that moves the branch
pointer. Finishing work is not consent to commit it. Read-only git — `status`, `log`, `diff`,
`show`, `fetch` — needs no permission.

## Running it

Open `index.html` in a browser. There is nothing to install, build, or serve — though a local
static server avoids `file://` quirks:

```bash
python3 -m http.server 8000     # then open http://localhost:8000
```

No test suite, no linter, no CI. Verify changes by loading the page and exercising the three
views (time machine, scroll, quiz).

## Rules

- Do not hand-edit `index.html`, and do not regenerate it with `build.py` (see above).
- Do not add a framework, bundler, or npm dependency without asking — see `docs/adr/`.
- Saved progress lives in `localStorage` under `time-tunnel-v2`. Changing that key's shape
  silently wipes every existing visitor's passport; bump the key instead.
- Canvas scenes are hand-drawn primitives, not images. Match the existing idiom
  (`paint`, `stroke`, `poly`, `ell`) rather than introducing a drawing library.
- `download` is a 0-byte file at the repo root. It is almost certainly `.nojekyll` mangled
  during upload — `.nojekyll` is required by Pages and is missing. Confirm, then add
  `.nojekyll` and delete `download`.
- Scenes are matched by **exact story title** in `SC` (line 1598). A title with no scene
  falls back to a generic landmark chosen by kind and era, so new stories still render.
