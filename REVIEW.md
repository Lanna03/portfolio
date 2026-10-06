# Review notes

## Task 1
- All work-card covers: 16:9 via `.work-card-img` (aspect-ratio 16/9, object-fit cover, center). `width`/`height` attrs are 1600x900 on all three imgs (Salud was 1920x1080, changed).
- KnowNow (1.26 source) cropped equally top/bottom with `center`; looks fine (phone and title intact), so `center 30%` was not needed.
- Card 1 scale: stays 1 at scrollY <= 8; `.is-inview` only when scrollY > 8 and intersecting (one passive scroll listener toggles card 1; cards 2/3 are IntersectionObserver-driven). No rAF special case exists in the file (already absent). prefers-reduced-motion disables the transform via CSS.
- Screenshots in `review-frames/`.
