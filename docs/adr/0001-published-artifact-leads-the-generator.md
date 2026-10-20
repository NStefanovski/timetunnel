# ADR-0001: The published `index.html` leads its own generator

**Decision**
`index.html` is treated as the source of truth for the live site, even though its first line
claims to be generated. It is the finished output of `build.py` + `stories.json` **plus** the
animated diorama, which was added to the output afterwards and which `build.py`'s template does
not yet produce.

**Do not** hand-edit `index.html` — a later build would discard the edit.
**Do not** run `build.py` either — a build today silently drops the diorama from the page.

**Reason**
The diorama (canvas engine, prop library, and a scene for each of the 262 stories) was built and
integrated directly into the published artifact. Until the template carries it, the generator
produces a strictly worse page than the one in this repository.

**Consequences**
- The three pieces the template needs ship in the handoff package, not here:
  `diorama.css` (goes in the main `<style>`; it also changes `.track-wrap`'s margin from
  `18px -16px 0` to `0 -16px 0`), `diorama-canvas.html` (immediately before
  `<div class="track-wrap">`), and `diorama-script.html` (a new `<script>` **after** the app's
  main script, before `</body>`).
- That load order is load-bearing: the diorama reads `track`, `stations`, `centers`, `layout`,
  `S`, `G`, `LN` and `line` from the main script (lines 495–496 and around).
- Preserving those three files somewhere version-controlled — ideally beside `build.py` — is the
  cheapest way to stop this divergence becoming permanent. They exist in one zip today.
- Until then, neither a content change nor a rebuild has a safe route in this repository. Say so
  rather than editing the artifact.
