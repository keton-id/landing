# keton.id — landing

Single-page landing site for keton.id. Svelte 5 + Vite, deployed as static site (CNAME → `keton.id`).

## Stack

- **Svelte 5.55** (runes mode — uses `$state()` for reactivity, see "Reactivity gotcha" below)
- **Vite 8** (`@sveltejs/vite-plugin-svelte` 7)
- Vanilla CSS (no framework, no preprocessor)
- Google Fonts: Inter + JetBrains Mono (loaded via `<svelte:head>`)
- Project icons via `cdn.simpleicons.org`

## Scripts

```bash
npm run dev      # vite dev server
npm run build    # vite build → dist/
npm run preview  # preview built site
```

## File map

```
src/
  App.svelte    # entire UI lives here (single-file app)
  app.css       # all styles, CSS custom properties for theming
  main.js       # mounts App to #app
  assets/
public/         # favicon.svg, static assets
index.html      # vite entry, references /src/main.js
CNAME           # GitHub Pages custom domain
```

No router. No component split — everything is in `App.svelte` + `app.css` by design (small surface, easier to edit).

## Component anatomy (`App.svelte`)

State (Svelte 5 runes):

- `theme` — `'dark' | 'light'`, persisted to `localStorage` under `keton-theme`
- `current` — active slide index in project ticker
- `paused` — pauses ticker auto-advance on hover
- `tickerTimer` — `setInterval` handle (cleared in `onDestroy`)
- `showLineup` — modal open state

Data (const arrays at top of `<script>`):

- `links` — top nav items
- `highlights` — chip strings under hero copy
- `stackIcons` — `{ Rust: { slug, color }, ... }` map for simpleicons lookup
- `projects` — array of `{ name, mark, markVariant, type, stack, desc, href }` — single source of truth for ticker + modal

Sections (top to bottom):

1. `.topbar` — brand + nav + theme toggle
2. `.hero` — 2-col grid:
   - Left (`.hero-copy`): eyebrow, h1, lede, chip-row, cta-row. Flex column with `justify-content: space-between` → stretches to right column height, content distributes evenly.
   - Right (`.hero-right`): vertical stack of `.terminal-card` (faux zsh) + `.project-ticker`.
3. `.project-ticker` (id `projects`) — auto-advancing slide (4s interval, hover-paused), dot nav, paper-flip animation (`@keyframes ticker-page` — rotateY hinge on left edge). Click slide → opens project URL in new tab.
4. `.signal-grid` (id `stack`) — 3 compact info cards (Mode / Style / Energy).
5. `.site-footer` — pinned to bottom via `margin-top: auto` on `.page-shell` flex column.
6. `.lineup-modal` (conditional) — opens when "See our lineup" CTA clicked. Backdrop blur + full project-card grid. Closes on backdrop click, ESC (`<svelte:window on:keydown>`), or × button.

## Reactivity gotcha (important)

This file uses **Svelte 5 runes mode** because top-level `let` reassignment inside `setInterval` callbacks was silently dropped in mixed mode. ALL reactive vars must use `$state()`:

```js
let current = $state(0)   // ✅ reactive
let foo = 0               // ❌ NOT reactive — template won't update
```

If you add new reactive state, declare it with `$state()`. Use `$derived()` for computed values.

## Theming

CSS custom properties on `:root` (dark, default) and `:root[data-theme='light']` (light override). `toggleTheme()` sets `data-theme` attr on `<html>` + persists choice. Both themes share variable names — to add a new color slot, define in both blocks.

Key tokens: `--bg-main`, `--text-main/soft/dim`, `--panel-bg/strong/border`, `--accent`, `--accent-2`, `--accent-glow`, `--shadow`, `--mono`.

Light theme uses gradient panel-bg + multi-radial bg + layered shadow with inset highlight for depth (avoiding flat / generic look).

## Layout strategy (no-scroll viewport)

`.page-shell` is `min-height: 100vh` flex column. `<main>` holds hero + signal-grid, footer pushed to bottom via `margin-top: auto`. Hero uses `align-items: stretch` so left & right columns match height. Sizes (h1 clamp, paddings) tuned so default content fits ~900px+ viewports without vertical scroll.

If adding content, expect this no-scroll budget to break — either compress further or accept scroll.

## Project ticker details

- Auto-advance every 4000ms (`setInterval` in `onMount`)
- `mouseenter` → pause, `mouseleave` → resume
- Manual dot click sets `current` directly
- Slide transition: `@keyframes ticker-page` — paper-flip effect via `rotateY(-105deg → 0deg)` with brightness shading and overshoot. Hinge is left edge (`transform-origin: left center`), perspective 1600px on parent.
- Respects `prefers-reduced-motion` (animation disabled).

## Adding a new project

Edit `projects` array in `src/App.svelte`. If using a new tech in `stack`, add it to `stackIcons` map (find slug at simpleicons.org). New project appears automatically in:

- Ticker rotation
- Lineup modal grid

Optional: add `.project-mark--<markVariant>` rule in `app.css` for custom mark styling (see existing `--jirac`, `--cora`).

## Deployment

Static build (`npm run build` → `dist/`) served from GitHub Pages. `CNAME` file pins custom domain.

## Conventions

- No emojis in code unless intentional product copy (e.g. `cora` mark `🤫`).
- No comments unless explaining non-obvious WHY.
- All UI strings in `App.svelte` (no i18n layer).
- Indonesian/English mixed copy is intentional — keton.id audience.
