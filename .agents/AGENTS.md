# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static Thai food recommendation site (ลิ้มรสไทย). Two HTML pages, one shared data file, no build step, no package manager, no test runner:

- `index.html` — catalog of 30 dishes grouped by category, with search, category tabs, spice-level filter, favorites, and a "spin for a random dish" slot-machine animation.
- `detail.html` — per-dish detail page (name, description, spice level, ingredients), reached via `detail.html?id=<dish-id>`.
- `js/dishes.js` — defines the single global `DISHES` array (the Catalog) consumed by both pages via a plain `<script src="js/dishes.js">` tag (not a module).
- `image/` — local hotlinked photo assets for dishes that don't have a usable Wikimedia Commons URL.

## Running / verifying changes

There is no dev server, bundler, or test suite. To view changes, open `index.html` (or `detail.html?id=...`) directly in a browser, or serve the folder locally, e.g. `python3 -m http.server`. Sanity-check JS with `node --check js/dishes.js` after editing dish data.

## Architecture notes

**Single source of truth for content:** All dish content (name, category, description, spice level, photo URL, emoji fallback, ingredients) lives in the `DISHES` array in `js/dishes.js`. Both pages read from this same array — there is no per-page copy of dish data.

**Favorites logic is duplicated, not shared:** `getFavorites`/`toggleFavorite` (backed by `localStorage["thaiFoodFavorites"]`, an array of dish ids) are implemented independently in the inline `<script>` of `index.html` and of `detail.html`. When changing favorites behavior, update both files — there is no shared JS module to factor it into (`js/dishes.js` holds data only).

**Photo fallback pattern:** Every dish has both a `photoUrl` (nullable) and an `emoji`. Rendering code always emits the emoji `<div>` (hidden via inline style if a photo exists) plus, when `photoUrl` is set, an `<img>` with `onerror` that removes itself and un-hides the emoji div. Follow this same pattern for any new place a dish photo is rendered.

**HTML escaping is selective, not blanket:** Dish fields (`name`, `description`, `category`) are trusted static content baked into `js/dishes.js` and are interpolated into template-literal HTML unescaped. Only `ingredients` text and the live search query are passed through the local `escapeHtml()` helper (duplicated in both pages) before insertion, since those paths were previously flagged for output-encoding issues. If you add any new field sourced from something other than the trusted `DISHES` array (or reuse an existing field somewhere new that touches user input), escape it the same way.

**Navigation, not SPA routing:** Clicking a catalog/result card does a full navigation to `detail.html?id=<id>` (`window.location.href`); there is no client-side router.

**Terminology:** `CONTEXT.md` is the authoritative glossary (Dish, Catalog, Category, Favorites, Spice Level, Spin) — match its Thai/English terms exactly in any new UI copy or code comments, and check `_Avoid_` entries before introducing new wording.

**Design system:** Both pages share the same inline `:root` CSS variables (cream/gold/teal/maroon palette, `Mitr`/`Taviraj` Google Fonts) copy-pasted into each `<style>` block — there is no shared stylesheet. Keep new styling consistent with these tokens rather than introducing new colors/fonts, and mirror any palette change into both files.
