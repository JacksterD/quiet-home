# Quiet Home

A calm, self-hosted browser homepage. No feeds, no algorithm, no tracking — just
the time, a greeting, a quiet search box, a small set of links you choose, and a
rotating human quote. It's a deliberate antidote to the attention economy: a page
that asks nothing of you when you open a new tab.

The guiding idea is a *smaller web* — doors you walk through on purpose, rather than
a feed that decides what you see. The aesthetic borrows from Dieter Rams ("less, but
better") and a dark, Farrow & Ball-ish palette.

## What it is, technically

- A **single self-contained `quiet-home.html` file**. No build step, no framework,
  no package manager, no dependencies.
- **Vanilla** HTML, CSS and JavaScript only.
- **Zero network calls.** No web fonts, no CDN, no favicons fetched from third
  parties, no analytics. The grain texture is an inline SVG data URI; fonts are
  system fonts. This is a feature, not a shortcut — it keeps the page instant,
  private, and silent.

## Features

- **Clock** showing time (24h) and full date. Refreshes by the minute on purpose —
  no ticking seconds, to avoid manufacturing urgency.
- **Time-aware greeting** ("Good morning/afternoon/evening", "Still up" after
  midnight), personalised with the user's name.
- **Quiet search** — a minimal underline field that submits to a chosen engine.
  Privacy-respecting options included: DuckDuckGo (default), Kagi, Startpage,
  Brave, Ecosia, and Google.
- **Curated links** — a small row of text links ("your web"), no icons.
- **Rotating quotes** — one human, attributed quote shown at random each load. The
  author is rendered as a quiet upright credit after the line.
- **Edit panel** — an unobtrusive "edit" control (bottom-right) opens a settings
  modal to change name, search engine, links, and quotes. Closes on Escape or
  backdrop click.

## Design language

- **Palette:** a deep blue-green near-black (Farrow & Ball Inchyra/Hague territory)
  with a single warm brass accent. Defined as CSS variables at the top of the file.
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
  the real links/quotes/name straight into the `defaults` object and host that; the
  edit panel is then optional per-device tinkering.

## Current defaults

- **Name:** Jack
- **Search:** DuckDuckGo (default); also Kagi, Startpage, Brave, Ecosia, Google.
- **Links:** Mail (Gmail), Calendar (Google Calendar), Notion, Wikipedia.
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

## Roadmap / next steps

- [ ] Initialise a git repo and push to GitHub.
- [ ] Bake the canonical name, links, search engine and quotes into `defaults`.
- [ ] Publish as a static site — GitHub Pages (ideally on a custom subdomain such as
      `home.jackd.org`), or Netlify Drop / Cloudflare Pages.
- [ ] Set as browser homepage / new-tab page; on iOS, "Add to Home Screen" for an
      app-like launcher.

### Optional future ideas

- **Config export/import** (copy a JSON blob, or a URL-encoded config) to move a
  setup between devices without retyping — the cleanest answer to the per-device
  `localStorage` limitation.
- **Light/dark or seasonal palette variants** via a CSS variable swap.
- **Auto-focus the search field** on load for keyboard-first use.
- A small **keyboard shortcut** to open the edit panel.

## Running locally

It's a static file: open `quiet-home.html` in any browser, or serve the folder with
anything (e.g. `python3 -m http.server`). No setup required.
