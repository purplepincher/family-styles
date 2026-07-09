# family-styles — the shared CSS design system for the PurplePincher domain family

`family-styles` is a small, **pure-CSS** design system: a handful of CSS
custom-property tokens and component class definitions, plus one HTML
partial. It exists so that the family of PurplePincher web properties
(`purplepincher.org`, `deckboss.net`, `cocapn.com`, `cocapn.ai`, …) can
share one visual baseline and one set of honesty components without each
site reinventing them.

There is **no JavaScript, no build pipeline, no runtime fetch, and no
component framework** in this repo. It is just CSS (and one HTML
fragment) that other sites inline into their own `<style>` blocks at
build time. If you are looking for a JS component library, this is not
that.

> **Status at a glance**
> - ✅ **The CSS itself is real and complete.** Every token and class
>   documented below exists in the source files; this README was written
>   by reading those files.
> - ✅ **`example/demo.html`** is a real, viewable page that exercises
>   the classes — open it in a browser to see the system.
> - ⚠️ **Per-site accent variables** (`--antifoul`, `--depth`, `--reef`)
>   are defined here but only take effect when a *consuming* site
>   overrides `--claw`. No consuming site lives in this repo.
> - 🔮 **`--reef` (superinstance.ai)** is defined but likely unused —
>   see the caveat below.
> - 🔮 **No automated test suite.** There is nothing to run; correctness
>   is verified by reading the CSS and by rendering the demo. See
>   "Testing" below.

## What's in here

| file | what it is |
|---|---|
| `tokens.css` | The shared `:root` custom-property palette: the ground palette + type system, plus named per-site accent variables (`--antifoul`, `--depth`, `--reef`). |
| `base.css` | The shared reset, type scale, and the load-bearing component shapes: `.eyebrow`, `.chain`/`.chain-step`, `.ledger`/`.ledger-row`, `.demo-card`, `.bar-list`. |
| `provenance-panel.css` + `provenance-panel.html` | The "what's real here" honesty component (persistent status line + per-element provenance tag). Drop-in partial for sites that need honesty framing. |
| `example/demo.html` | A standalone demo page that links the two CSS files and renders every component class, so you can see the system in a browser. |

The published npm package (`@purplepincher/family-styles`, see
`package.json`) ships exactly the four asset files (`tokens.css`,
`base.css`, `provenance-panel.css`, `provenance-panel.html`); the
`example/` directory and this README are repo-only.

## Quick start

### Just look at it (no install)

Open the demo in any browser:

```bash
# from the repo root
open example/demo.html      # macOS
xdg-open example/demo.html  # Linux
# or simply: open the file in your browser
```

`example/demo.html` `<link>`s `../tokens.css` and `../base.css`, so it
renders the real palette and the real component classes with nothing
else installed. It uses the default aubergine `--claw` accent.

### Use it in a site

Install the package (it only contains static CSS/HTML):

```bash
npm install @purplepincher/family-styles
# or, equivalently, copy the four files out of node_modules/@purplepincher/family-styles/
```

Then, in your site's build step, concatenate the shared CSS into your
site's own `<style>` block and the shared HTML partial into the body.

## How a family site uses this

**Inline at build time. Never fetch at runtime.**

The build-time-inline pattern is the load-bearing architectural
decision. Concretely, each site's build step concatenates the shared CSS
into that site's own `<style>` block (and the shared HTML partials into
the body) before the Worker is deployed. The deployed artifact is then a
self-contained HTML document — identical in shape to a single-page Worker
that serves HTML imported as a `Text` module, **CSP unchanged** (no
provision for a shared CSS host, no runtime fetch, no new origin in the
perimeter).

Why inline rather than `<link>` to a hosted stylesheet:

- A strict CSP with `style-src 'self' 'unsafe-inline' ...` and **no**
  provision for a shared stylesheet host is part of the honesty
  architecture for these sites, not a nicety.
- Adding a shared CSS host to the CSP would weaken that perimeter for no
  real benefit.
- Inlining keeps each deployed site hermetic — exactly the property
  that makes these sites checkable ("the page IS the real HTML, nothing
  fetched").

> 🔮 **Note:** this repo contains **no build script**. The intended
> consuming-site build step (a `build.mjs` that reads `tokens.css` +
> `base.css` (+ `provenance-panel.css` when needed), reads the site's
> own HTML template, and concatenates the shared `<style>` into the
> site's `<style>` block) lives in **each consuming site**, not here.
> The "≈30 lines of Node" described in the original plan is a pattern a
> consuming site follows, not an artifact shipped by this repo.

## Per-site accent swap

Each family site changes exactly one accent variable and adds exactly
one signature interactive surface. The accent swap is done in the site's
own `:root`, overriding `--claw`:

```css
/* deckboss.net — site-level :root, after tokens.css is inlined */
:root {
  --claw: var(--antifoul);   /* anti-fouling bottom-paint oxide */
}
```

All existing `var(--claw)` references in `base.css` then pick up the
site's chosen accent automatically. The named accents:

| site | accent | variable |
|---|---|---|
| purplepincher.org | aubergine (reference, unchanged) | `--claw` (default) |
| deckboss.net | anti-fouling oxide | `--antifoul` `#B0533A` |
| cocapn.com | the water under the hull | `--depth` `#1F4A5C` |
| cocapn.ai | lattice cyan promoted to primary | `--lattice` (no new token) |
| superinstance.ai | warm coral / sand-rose | `--reef` `#D97A5C` *(see caveat)* |
| capitaine (placeholder) | `--depth` family, or TBD when real | TBD |

**superinstance.ai caveat:** `--reef` was named for superinstance.ai,
but superinstance.ai already has its own real, live visual identity on
Cloudflare Pages (a green-on-black terminal aesthetic) that differs from
this family skeleton. `--reef` is kept defined here for consistency with
the original plan, but superinstance.ai will likely keep its existing
palette rather than adopt the skeleton. If that decision is confirmed
permanent, `--reef` can be removed in a later round.

## Variable reference (`tokens.css`)

Ground palette + type (the fixed visual baseline for every family site):

- `--ink` `#0E1A1C`, `--ink-deep` `#081113` — tidepool-teal backgrounds
- `--shell` `#F0E9D8`, `--shell-dim` `#C9C0AB` — warm bone foregrounds
- `--claw` `#A8548C`, `--claw-deep` `#7A3B5E` — purplepincher's aubergine (overridden per site)
- `--lattice` `#5FB4C7`, `--lattice-dim` `#3F8FA3` — the precision/lattice half of the duality
- `--hardened` `#D98B54` — the "earned the shell" highlight
- `--line` `rgba(240, 233, 216, 0.14)` — hairline divider color
- `--display` Fraunces, `--body` IBM Plex Sans, `--mono` IBM Plex Mono

Per-site accents: `--antifoul`, `--depth`, `--reef` (see table above).

## Class reference (`base.css`)

| class | purpose |
|---|---|
| `.wrap` | 920px content container with 28px gutters |
| `.eyebrow` | mono uppercase section label (lattice-colored) |
| `.chain` + `.chain-step` (+ `.chain-num`) | numbered horizontal sequence; order carries meaning |
| `.demo-card` | interactive-surface shell (grid: primary area + 260px sidebar); each site fills it with its signature surface |
| `.ledger` + `.ledger-row` (+ `.flagship`, `.ledger-name`, `.ledger-desc`, `.ledger-tag`, `.ledger-tag.hardened`) | registry of named things with optional tag |
| `.bar-list` | numbered criteria list with auto-counter |

`base.css` also includes a reset, base type scale, focus-visible
outlines, `::selection` styling, a `prefers-reduced-motion` fallback
that disables smooth scroll, and a couple of responsive breakpoints
(`.demo-card` collapses to a single column under 680px; `.ledger-row`
collapses under 620px).

## Provenance panel reference

Two pieces (see `provenance-panel.html` for the drop-in template):

- `.provenance-status` (+ `__word`, `__text`) — sticky persistent status line; one per page, near top of `<body>`. States the site's overall reality in one line. The persistence is the mechanism.
- `.provenance` (+ `--block` modifier) — inline provenance tag, attached to any element whose realness a visitor needs to know. Repeatable.

**Use on:** cocapn.com, cocapn.ai (and any future site whose load-bearing claim is "real but not yet a product" or "honest about what it is").
**Do NOT use on:** deckboss.net (real shipped product — the framing implies doubt where there is none), purplepincher.org.

## Testing

There is **no automated test suite** in this repo, and `package.json`
defines no scripts. Correctness is established two ways:

1. **By reading the source** — the tokens and classes are simple CSS;
   the references above were checked against the files line by line.
2. **By rendering** `example/demo.html`, which exercises `.chain`,
   `.ledger`, `.ledger-row`, `.flagship`, `.ledger-tag`, and `.eyebrow`.

If you change a token or class, update the demo (or add a consuming
site's page) so the change is visible. There is no CI that will catch a
broken selector for you.

## What this directory is NOT

- Not a component library or framework — just raw CSS files (and one HTML partial).
- Not fetched at runtime. There is no runtime fetch path; the shared CSS exists only as build-time input.
- Not a monorepo config. Each site is its own Worker, its own `wrangler.jsonc`, its own route, its own deploy command. There is deliberately no `deploy-all`.
- Not the place to invent new identities. The capitaine identity, the deckboss.ai-vs-.net canonical question, the cocapn.ai-vs-.com merge question, and the lucineer question are all still open and out of scope here.

## License

MIT — see [LICENSE](./LICENSE).
