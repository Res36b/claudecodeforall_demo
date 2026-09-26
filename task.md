# Task List — Thai Food Recommendation Site

Derived from `CONTEXT.md` (glossary) and the confirmed design. Deliverable: a single self-contained `index.html` (inline CSS/JS, no build step, no external files besides hotlinked photos).

## 1. Content research (Catalog)
- [x] Curate 30 real, well-known Thai Dishes, split ~7–8 per Category (ต้ม / ผัด / แกง / ทอด)
- [x] For each Dish, write: Thai name, one-line Thai description, Spice Level (1–5 chili scale)
- [x] For each Dish, find a free-licensed photo URL (e.g. Wikipedia Commons); mark dishes with no good photo for emoji fallback — 14/30 got a real Wikimedia photo (all verified HTTP 200), remaining 16 use emoji fallback
- [x] Assemble the 30 Dishes into a single JS data array/object baked into the page

## 2. Page skeleton & theme
- [x] Create `index.html` with inline `<style>` and `<script>` (no separate files)
- [x] Set up ผ้าไทยร่วมสมัย (contemporary-traditional) theme: muted gold/teal palette, Thai web font (e.g. Kanit/Mitr/Taviraj via Google Fonts), subtle Thai pattern accents/borders
- [x] Build responsive layout: hero (Spin button) → Catalog (grouped by Category) → Favorites section
- [x] All UI copy in Thai only

## 3. Spin feature
- [x] Build "สุ่มเมนูอาหารไทยวันนี้" button
- [x] Implement slot-machine-reel animation (~2s) cycling Dish names/icons
- [x] On stop, reveal a result card for one Dish, fully randomized from the whole Catalog on every click (no date-seeding, no persistence across reloads)
- [x] Result card shows: photo/fallback emoji, name, description, Spice Level, Favorite toggle

## 4. Catalog display
- [x] Render all 30 Dish cards grouped under their 4 Categories
- [x] Each card shows: photo (fallback emoji if none found), name, description, Spice Level (chili icons), Favorite toggle
- [x] Photos use external hotlinks; verify graceful fallback (emoji) when a Dish has no photo URL — `onerror` swaps to the emoji div; all 14 photo URLs curl-verified 200 OK

## 5. Favorites feature
- [x] Add Favorite (heart/star) toggle to every Catalog card and the Spin result card
- [x] Persist the set of favorited Dish IDs to `localStorage`
- [x] On load, restore toggle state from `localStorage` and reflect it on all matching cards
- [x] Build "รายการโปรดของฉัน" section/filter that lists only favorited Dishes, updating live as toggles change

## 6. QA
- [x] Verify favorites persist across page reload — `toggleFavorite` writes array to `localStorage['thaiFoodFavorites']`; render functions read it fresh on every call, including after reload
- [x] Verify Spin gives a fresh random result on every click, no repeats of persisted "today" state — `finalDish` is re-picked with `Math.random()` on every click, nothing written to storage for spin results
- [x] Verify layout on mobile width — responsive grid (`auto-fill, minmax(220px,1fr)`) and a `@media (max-width:480px)` rule for the spin button
- [x] Verify text/data renders instantly even if photo hotlinks are offline/broken (emoji-only degrade, no layout breakage) — `<img onerror>` removes the image and reveals the emoji div; text/data has no network dependency at all
- [x] Check all UI copy against `CONTEXT.md` glossary for consistent terminology (เมนู/Dish, ประเภท/Category, รายการโปรด/Favorites, ระดับความเผ็ด/Spice Level) — consistent throughout
- [x] Automated checks: JS syntax validated (`node --check`), all 30 dishes parse with unique ids, correct 8/8/7/7 category split, spice levels in 1–5, all 14 photo URLs curl-verified HTTP 200
- [ ] Manual visual/interactive check in an actual browser — Chrome extension not installed this session; recommend opening `index.html` directly to confirm animation feel and visual theme
