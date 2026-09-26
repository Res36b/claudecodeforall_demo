# Thai Food Recommendation Site

A single-page static website (`index.html`) that recommends Thai dishes, lets visitors spin for a random suggestion, and save favorites locally in the browser.

## Language

**Dish** (เมนู):
One food item in the catalog, e.g. "ต้มยำกุ้ง". Colloquial Thai uses "เมนู" for this even though it literally means "menu" — we use it the same way in the UI, but "Dish" is the precise term for documentation.
_Avoid_: Menu item (in docs), item

**Catalog** (รายการอาหารทั้งหมด):
The full set of 30 curated Dishes shown on the page, researched once and baked into the page at build time (not fetched live at runtime).
_Avoid_: Menu (ambiguous with Dish), list

**Category** (ประเภท):
One of the four cooking-method groupings a Dish belongs to: ต้ม (boiled), ผัด (stir-fried), แกง (curry), ทอด (fried). Every Dish belongs to exactly one Category.

**Favorites** (รายการโปรด):
The subset of Dishes a visitor has marked as liked. Persisted per-browser via localStorage; has its own dedicated view/filter on the page, separate from the full Catalog.
_Avoid_: Likes, saved items

**Spice Level** (ระดับความเผ็ด):
A per-Dish attribute (chili-icon scale) indicating how spicy that Dish typically is, shown on its card.

**Spin** (สุ่มเมนู):
The act of triggering the slot-machine-reel animation to reveal one randomly chosen Dish from the Catalog. Fully ephemeral — "today" in the button's label is flavor text only; every Spin is independently randomized with no relationship to the calendar date, and results are not persisted across reloads.
_Avoid_: Today's pick (implies date-based persistence, which this does not have), draw
