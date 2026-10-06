# peel-and-cards — Review

All checks passed. No NOT DONE items.

---

## Task 1 — Work-card images: uniform 16:9 crop

**All three cards: 960×540 at 1728×1000 viewport (aspect ratio 1.778).**

Verification numbers (Playwright @ 1728×1000, networkidle):

| Card | offsetWidth | offsetHeight | aspectRatio |
|------|-------------|--------------|-------------|
| Salud | 960 | 540 | 1.778 |
| KnowNow | 960 | 540 | 1.778 |
| FloatOS | 960 | 540 | 1.778 |

**KnowNow fix decision:**  
The original `<picture>/<source>` setup loaded `Knownow_cover-2400.webp` (2400×1905, ratio 1.26). When a `<source>` is selected, the browser adopts the source's actual pixel dimensions as the `<img>`'s intrinsic size, overriding the HTML `width`/`height` attributes. This caused `height:auto` to resolve to 762 px (1.26 ratio) despite `aspect-ratio:16/9` in CSS — because for replaced elements with a known intrinsic ratio, the intrinsic ratio takes priority over CSS `aspect-ratio`.

Fix: removed the `<picture>/<source>` wrapper; switched to `<img srcset="images/Knownow_cover-2400.webp 2400w">` directly on the `<img>`. With `<img srcset>`, HTML `width="1600" height="900"` remain authoritative for intrinsic-size computation, so `height:auto` yields 540 px. WebP delivery is preserved (`currentSrc` confirms `Knownow_cover-2400.webp` loads in Chrome).

**KnowNow `object-position`:** `center` (the default) looks correct — the image crops symmetrically top and bottom. No adjustment needed.

---

## Task 2 — Card 0 (Salud) alignment at page top

| Condition | card0Left | h1Left | scale | is-inview | is-attop |
|-----------|-----------|--------|-------|-----------|---------|
| scrollY=0 (load) | 374 | 374 | 1.000 | false | false |
| scrollY=40 | — | — | 0.964 | true | false |
| scrollY=300 | — | — | 0.949 | true | false |
| scrollY=800 | — | — | 0.943 | true | false |
| scrollY=0 (back) | 374 | 374 | 1.000 | false | **true** |

`transform` values:
- scroll=40: `matrix(0.964154, 0, 0, 0.964154, 0, 0)`
- scroll=300: `matrix(0.949431, 0, 0, 0.949431, 0, 0)`
- scroll=800: `matrix(0.943363, 0, 0, 0.943363, 0, 0)`
- back to 0: `matrix(1, 0, 0, 1, 0, 0)` ← instant snap, no transition delay

**`is-attop` mechanism:** when `scrollY ≤ 8`, the scroll listener adds `.is-attop` to `#work-card-0` and removes `.is-inview`. CSS rule `#work-card-0.is-attop { transition: none; }` (higher specificity than `.work-card` and `.work-card.is-inview`) suppresses the 0.9s out-transition, so the card snaps instantly to `scale(1)` with `card0Left=374`.

Screenshots: `review-frames/01-scroll0-before.png`, `review-frames/02-scroll800.png`, `review-frames/03-scroll0-back.png`
