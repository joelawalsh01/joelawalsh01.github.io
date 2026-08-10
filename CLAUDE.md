# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Dr. Joel A. Walsh's personal academic site, served as a GitHub Pages **user site** (`joelawalsh01.github.io`) directly from the repo root on `master`. Pure static HTML/CSS/JS — no package.json, no build step, no tests, no CI.

- **Preview locally:** `python3 -m http.server 8000` from the repo root, then open `http://localhost:8000`.
- **Deploy:** `git push origin master`. There is no staging environment; a push is a publish.

## Two independent design systems

Do not assume a change in one applies to the other.

**1. Industrious template pages** — `index.html`, `aboutme.html`, `contact.html`, `presentations.html`, plus unused leftovers `generic.html`, `elements.html`, `elements copy.html`.

They load `assets/css/main.css` then `assets/css/overrides.css`, and the five scripts in `assets/js/` (jQuery + template helpers).

- `assets/css/main.css` is the vendored, light-themed Industrious build compiled from `assets/sass/`. **Never edit `main.css` or the Sass sources** — no Sass compiler is configured in this repo, so `.scss` edits would not reach the site.
- `assets/css/overrides.css` is the only stylesheet to edit. It repaints the light template into a dark "Shadowrun Highland Park" theme via `:root` custom properties (`--bg-primary`, `--accent` teal, `--accent-warm` amber, etc.) and leans on `!important` to beat `main.css`. Add new styling by defining/reusing those tokens rather than hardcoding hex values.
- The banner (`#banner`) and subpage heading (`#heading`) both pull `images/highland_park_banner.png`; `#banner` is sized by `aspect-ratio` to show the full art, with `#name-bar` beneath it as the title strip.
- `overrides.css` requests "Computer Modern Typewriter", which is not bundled in `assets/fonts/` (that directory is FontAwesome only) — type falls back to Courier New.

**2. `m2l2.html`** — a fully standalone page for the Multimodal and Multilingual Learning Technologies Lab. Light theme, Google Fonts (Inter / Source Serif 4), all CSS in one inline `<style>` and all JS in one inline `<script>`. It shares nothing with `assets/` except files under `images/`. Its tab UI is a plain `.tab-btn[data-tab]` → `.tab-panel#id` toggle of the `active` class.

## Adding an M2L2 lab member

The dominant recent workflow. In `m2l2.html`, inside `#members`, append a `.member-card` block under the correct `.section-label` group — "Lab Members" vs "Affiliated Researchers" — following the existing card shape (`img` + `.member-info` with `h3`, `p.affiliation`, bio `p`). Portraits go in `images/` as `firstname_lastname.jpg`, cropped square (CSS renders them 120px circular with `object-fit: cover`). Bios typically arrive as PDF/DOCX/HEIC in `~/Downloads`; `sips` handles HEIC→JPEG conversion.

## Known quirks — do not "fix" as a side effect

- **Line endings are mixed.** `index.html` and `m2l2.html` are LF; `aboutme.html`, `contact.html`, `presentations.html`, `generic.html`, `elements*.html` are still CRLF. Match the file you are editing, or Edit-tool string matches will silently fail.
- **Legacy pages have unclosed markup.** `aboutme.html`, `contact.html`, and `presentations.html` end mid-content — the `.row`/`.content`/`.inner`/`section` elements are never closed before the `<script>` tags, and `index.html` carries an orphan `</footer>` plus stray `</li>`. Browsers recover; leave it unless the task is explicitly to repair the markup.
- **Navigation is hand-maintained per page and inconsistent.** `index.html`'s `#menu` lists Home + M2L2 Lab; the other template pages list only Home. Adding a page means editing each nav by hand.
- `elements.html`, `elements copy.html`, and `generic.html` are untouched template demos, unlinked from any nav.
- `CREDITS.txt` credits Unsplash/Coverr images from the original template, not the current site imagery.
