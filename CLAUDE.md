# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

"Akıl Stüdyo" is a single-page marketing site (Turkish, `lang="tr"`) for a business offering AI automation, smart assistants and team training. The entire site is one self-contained file: `index.html` (inline CSS + inline vanilla JS, no build step, no dependencies, no tests, not a git repo).

To preview, open `index.html` in a browser (e.g. `open index.html`) or serve the folder (`python3 -m http.server`).

## Structure of index.html

- **CSS** (`<style>` in `<head>`): design tokens live on `:root` (colors, three font families, spacing scale `--s1`…`--s6`, `--r`, `--max`). Dark mode redefines the color tokens twice — under `@media (prefers-color-scheme: dark)` guarded by `:root:not([data-theme="light"])`, and under `:root[data-theme="dark"]`. Any color token change must be made in all three places. Use tokens rather than hard-coded values.
- **Fonts**: Bricolage Grotesque (display), Instrument Sans (body), JetBrains Mono (labels/numbers), loaded from Google Fonts.
- **Markup** (`<main id="basla">`): hero with a live savings calculator, then sections linked from the nav by id — `#hizmetler`, `#surec`, `#senaryolar`, `#hakkimizda` (founder photo `kurucu.jpg`, kept next to index.html), `#paketler`, `#sss`, `#iletisim` (contact form). Sections use `aria-labelledby` pointing at their `<h2>`.
- **JS** (single IIFE at the bottom):
  - Savings calculator: reads range inputs `#hours`, `#cost`, `#rate`; computes monthly hours as `hours × 4.33 × rate`, then monthly/yearly ₺ savings; formats with `Intl.NumberFormat('tr-TR')` into `#r-hours`, `#r-month`, `#r-year`.
  - Contact form (`#contact-form`): validates name/email client-side, then POSTs `FormData` via `fetch` to the Formspree endpoint in the form's `action` attribute (`https://formspree.io/f/<id>`), with a `_gotcha` honeypot and `_subject` hidden field. While `action` still contains the placeholder `FORM_ID`, submit shows a "not configured" message instead of sending. Status/errors go to `#form-note` (`.is-error` for errors).

## Conventions

- All user-facing copy is Turkish; keep new copy in Turkish and use `₺` / `tr-TR` number formatting.
- Layout must work at phone width (nav links hide below 860px; side padding respects safe-area insets) and honor `prefers-reduced-motion`.
