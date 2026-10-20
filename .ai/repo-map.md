# Repo map

Three files: `README.md`, `index.html`, and a 0-byte `download` (almost certainly `.nojekyll`
mangled on upload). Everything is `index.html`, so this map is a **line index into that one
file**. The repo root is also the GitHub Pages publish root.

`index.html` is the finished artifact and is **ahead of its own generator** — do not edit it and
do not rebuild it. See `CLAUDE.md` and `docs/adr/0001-published-artifact-leads-the-generator.md`.

```bash
sed -n '649,812p' index.html                 # read one section
grep -n "pattern" index.html | cut -c1-110   # ALWAYS truncate — line 359 is ~300 KB
```

## Line index

| Lines | What |
|---|---|
| 1–5 | `<head>`, meta, Google Fonts (the only external dependency) |
| **6–294** | **`<style>`** — all CSS |
| 43 / 54 / 67 | header · line switch · tabs |
| 73 / 97 | time machine · the diorama canvas |
| 157 / 198 / 247 | feed · quiz · deeper content |
| **295–356** | **Markup** — header, `#machine`, `#feedView`, `#quiz`, bottom nav |
| **357–965** | **App script** |
| 358–396 | `DATA` — the entire corpus. **Line 359 is one ~300 KB JSON line.** `V1_TITLES` at 361 |
| 364–374 | `LINES` — the derived per-volume index built from `DATA` |
| 397–421 | Helpers: `yr`, `label`, `span`, `ico`, `deepHTML`, `useLine` |
| 422–456 | Saved progress — `localStorage` key `time-tunnel-v2`, `seen`/`mast`, `persist`, `stamp` |
| 457–470 | View switching (`show`), toast |
| 471–493 | Volume switcher |
| 494–648 | **Time machine** — track, stations, odometer, drag/scroll, scale, panel |
| 649–812 | **Feed** — `IntersectionObserver` infinite scroll, story cards, alt cards |
| 813–953 | **Quiz** |
| 954–965 | Start — initial view from `location.hash` |
| **967–3338** | **Diorama** — hand-rolled canvas engine on `#dio`, 12 fps "boil" |
| 968–1001 | Context, palette, seeded randomness (`hash`, `rnd`, `jit`, `lerp`, `ease`) |
| 1002–1058 | Primitives: `poly`, `ell`, `trace`, `paint`, `stroke`, `glow`, `dot`, `flag`, `smoke`, `sparks` |
| 1059–1318 | `L` — 28 landmarks, one per station (`pyramid`…`serpent`; myths from 1286) |
| 1351–1569 | Prop library — people 1381, animals 1422, nature 1466, buildings 1490, objects 1516, vessels 1531 |
| 1570–1597 | Story-scene preamble |
| **1598** | **`SC`** — scenes keyed by **exact story title**; an unmatched title falls back to a generic landmark by kind and era |
| 1599–3189 | The scene bodies, grouped by volume (see below) |
| 3190–3222 | The traveler — the lantern-bearer who walks the timeline |
| 3223–3338 | World layers, paper grain, `requestAnimationFrame` loop |

### Story scenes by volume

| Lines | Volume |
|---|---|
| 1599 | History · Vol 1 — Ancient Times |
| 1874 | Vol 2 — The Middle Ages |
| 2112 | Vol 3 — Early Modern |
| 2288 | Vol 4 — The Modern Age |
| 2531 | Vol 5 — The 21st Century |
| 2678 | Myths · Greek gods & heroes |
| 2759 | Homer's tales |
| 2831 | The Bhagavad Gita |
| 2902 | Japanese myths |
| 2966 | Polynesian myths |
| 3039 | Norse myths |
| 3113 | African myths |

## Load order (load-bearing)

The diorama script (967–3338) reads globals defined by the app script (357–965): `track` and
`stations`/`centers` (495–496), plus `layout`, `S`, `G`, `LN` and `line`. It must stay **after**
the main script. Reordering the two `<script>` blocks breaks the diorama silently.

## Content shape

`DATA.lines` holds two volumes-of-volumes:

| Key | Groups | Stories | Facts |
|---|---|---|---|
| `history` | 5 | 178 | 10 |
| `myths` | 7 | 84 | 9 |

A story carries `year`, `group`, `kind`, `title`, `story`, `whoa`, and optionally `sides`
(a two-sided debatable question) and `approx`. Inspect it without printing the line:

```bash
node -e "const m=require('fs').readFileSync('index.html','utf8').match(/const DATA = (\{.*?\});?\n/s);
         const d=JSON.parse(m[1]); console.log(d.lines.history.stories[0])"
```

## Finding things

```bash
grep -n "function nextStory" index.html | cut -c1-110    # jump to a symbol
grep -nE "^\s{0,2}(const|let|function) " index.html | cut -c1-95   # all top-level symbols
grep -nE "^\s*/\*" index.html | cut -c1-95               # the file's own section banners
git log --oneline -10
```
