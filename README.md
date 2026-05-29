# Quiet Home

A calm, self-hosted browser homepage. No feeds, no algorithm, no tracking — just
the time, a greeting, a quiet search box, a small set of links you choose, and a
rotating human quote. It's a deliberate antidote to the attention economy: a page
that asks nothing of you when you open a new tab.

The guiding idea is a *smaller web* — doors you walk through on purpose, rather than
a feed that decides what you see. The aesthetic borrows from Dieter Rams ("less, but
better") and a dark, Farrow & Ball-ish palette.

## What it is, technically

- A **single self-contained `index.html` file**. No build step, no framework,
  no package manager, no dependencies. (Served as `index.html` so it works at a
  site root; it remains one droppable file.)
- **Vanilla** HTML, CSS and JavaScript only.
- **Zero network calls.** No web fonts, no CDN, no favicons fetched from third
  parties, no analytics. The grain texture *and the favicon* are inline SVG data
  URIs; fonts are system fonts. This is a feature, not a shortcut — it keeps the
  page instant, private, and silent.
- **Favicon** — the brass dot from "Hello." on the page's near-black, embedded as
  an inline SVG data URI in the `<head>`. No `.ico` file, no request. (SVG
  favicons render in current Chrome, Edge, Firefox and Safari; older browsers
  simply fall back to the default — a graceful, no-cost degradation.)

## Features

- **Clock** showing time (24h) and full date. Refreshes by the minute on purpose —
  no ticking seconds, to avoid manufacturing urgency.
- **Time-aware greeting** ("Good morning/afternoon/evening", "Still up" after
  midnight), personalised with the user's name.
- **Quiet search** — a minimal underline field that submits to a chosen engine.
  Privacy-respecting options included: DuckDuckGo (default), Kagi, Startpage,
  Brave, Ecosia, and Google. The field is **focused on load** (so you can type
  immediately) and is **URL-aware**: type a bare web address like `github.com`
  and Enter goes straight there, while anything with spaces (or no domain) is
  searched. Note: a web page cannot focus the *browser's* address bar — that's
  reserved to the browser — so focusing this field is the in-page equivalent.
- **Bang routing** — prefix a single query with a `!bang` to send just that one
  query somewhere other than your default, without changing the default. No bang
  (or an unrecognised one) falls through to the chosen engine. Supported:
  `!ddg`, `!kagi`, `!sp`, `!brave`, `!e`, `!g`, plus AI: `!gpt` (ChatGPT). Each
  bang is just a URL prefix the query is appended to — a plain navigation, no API
  calls or keys, so the zero-network model is preserved. Note that, like any
  search, the query text does travel to whichever provider you send it to (and AI
  vendors may handle prompts differently from a privacy search engine), so routing
  to AI is a deliberate, per-query choice — which is the point. (A `!claude` bang
  was trialled but removed: Claude.ai now shows a session-hijack warning on any
  externally-supplied `?q=` prefill — a deliberate anti-prompt-injection measure —
  which makes it a poor fit for a one-keystroke launcher.)
- **Curated links** — text links ("your web"), no icons. A single centered line
  (middot-separated) is always shown; a quiet "more" toggle eases the rest open on
  one further line, collapsed by default. No labels — just the links. They are
  baked into the file (the `PRIMARY_LINKS` and `MORE_LINKS` arrays near the top of
  the `<script>`) and edited there by hand, not from the edit panel.
- **Rotating quotes** — one human, attributed quote shown at random each load. The
  author is rendered as a quiet upright credit after the line. The quotes are baked
  into the file (the `notes` array in `defaults`) and edited there by hand.
- **Edit panel** — an unobtrusive "edit" control (bottom-right) opens a settings
  modal to change name, search engine, and palette. Closes on Escape or backdrop
  click. (Links and quotes are baked in, so they're not in this panel.)

## Design language

- **Palette:** three quiet, switchable palettes, each defined as CSS variables at
  the top of the file and keeping a single warm brass-family accent:
  **Night** (a deep blue-green near-black, Farrow & Ball Inchyra/Hague territory —
  the default), **Day** (a warm paper/parchment light mode with deep studio-green
  ink), and **Ember** (a cozy warm-dark evening variant). The choice is set as
  `data-theme` on `<html>`, swaps only the colour variables, is remembered in the
  config, and is applied before first paint so a light theme never flashes dark.
  Pick it from the edit panel (it previews live as you choose).
- **Type:** a system serif stack ("Iowan Old Style"/Palatino/Georgia) for the
  greeting and quotes; a system mono ("SF Mono"/ui-monospace) for the small
  letter-spaced labels and clock. No fonts are downloaded — on Apple devices this
  renders as New York / SF Mono.
- **Atmosphere:** a soft radial vignette plus a very faint inline-SVG film grain.
- **Motion:** one gentle staggered fade-in on load. Nothing else moves.

## Data & persistence

- All config lives in **`localStorage`** under a single key: `quiet_home_v1`.
- On first load (or in any browser with nothing stored), the page falls back to the
  `defaults` object near the top of the `<script>`.
- **Important:** `localStorage` is per-origin and per-device. Hosting the page once
  does **not** sync edits between, say, a Mac and an iPhone — each browser keeps its
  own copy. The intended pattern for a single canonical setup is therefore to bake
  the real quotes/name straight into the `defaults` object (and the links into
  `PRIMARY_LINKS`/`MORE_LINKS`) and host that; the edit panel is then optional
  per-device tinkering.

## Current defaults

- **Name:** Jack
- **Search:** DuckDuckGo (default); also Kagi, Startpage, Brave, Ecosia, Google.
- **Links:** primary line Calendar · Mail · Notes (always shown); "more" reveals
  Wikipedia · Internet Archive · Small Web · Weather · Maps · Transport.
- **Quotes:** nine attributed human quotes (Rams, Mary Oliver, Annie Dillard,
  Georgia O'Keeffe, Dorothea Lange, Thoreau, David Lynch, Tim Berners-Lee,
  Simone Weil).

## Constraints worth protecting

If extending this, please keep the things that define it:

1. **Single file, no dependencies, no build step.** It should always be droppable as
   one `.html` onto any static host.
2. **Zero network calls.** No external fonts, scripts, analytics, or icon fetches.
3. **No attention-capture patterns.** No feeds, no notifications, no infinite content,
   no engagement metrics, no autoplaying anything.
4. **Privacy-first.** Nothing should leave the device. Config stays in local storage.
5. **Restraint over features.** When in doubt, leave it out. The point is calm.

## Deployment

Published as a static site via **GitHub Pages** on the custom subdomain
**`home.jackd.org`**. The `CNAME` file in the repo root sets the custom domain;
the matching DNS record is a `CNAME` for `home.jackd.org` →
`jacksterd.github.io`. Pages serves `index.html` at the site root, so the page
loads directly at the subdomain with no path suffix.

## Roadmap / next steps

- [x] Initialise a git repo and push to GitHub.
- [x] Bake the canonical name, links, search engine and quotes into `defaults`
      (the shipped `defaults` are the canonical setup).
- [x] Publish as a static site — GitHub Pages on the custom subdomain
      `home.jackd.org`.
- [ ] Set as browser homepage / new-tab page; on iOS, "Add to Home Screen" for an
      app-like launcher.

### Optional future ideas

- **Config export/import** (copy a JSON blob, or a URL-encoded config) to move a
  setup between devices without retyping — the cleanest answer to the per-device
  `localStorage` limitation.
- A small **keyboard shortcut** to open the edit panel.

## Running locally

It's a static file: open `index.html` in any browser, or serve the folder with
anything (e.g. `python3 -m http.server`). No setup required.
