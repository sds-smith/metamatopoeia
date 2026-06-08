# Research: Bug 005-nav-glass-blur-mobile

**Branch**: `005-nav-glass-blur-mobile`
**Phase**: 0 (Pre-implementation research)

---

## R-1: `animation-timeline: scroll()` on Pseudo-Elements in Safari 17.4+

**Decision**: Proceed with `animation-timeline: scroll(root)` on `.header::before`.

**Rationale**: CSS Scroll-Driven Animations Level 1 spec defines pseudo-elements as valid animation targets. Safari 17.4 (March 2024, WebKit 17.4) shipped the full specification including pseudo-element support. No known exclusions. The `::before` pseudo-element will correctly receive the scroll timeline.

**Graceful degradation**: If a browser does not support scroll-driven animations on pseudo-elements, `animation-fill-mode: both` + `from { opacity: 0 }` ensures `::before` remains invisible (transparent nav). This is identical to the graceful degradation behavior of the original implementation for browsers that don't support `animation-timeline: scroll()` at all — not a regression.

**Alternatives considered**:
- _Class-toggle via JS scroll event_: Would bypass the scroll-driven animation concern entirely; rejected by user preference (CSS-only approach preferred).
- _`@scroll-timeline` (legacy syntax)_: Deprecated; not applicable.

---

## R-2: `opacity: 0` + `backdrop-filter` Bleed-Through on WebKit

**Decision**: Use `opacity: 0` as the initial state of `.header::before` without additional suppression.

**Rationale**: A historical WebKit bug (prior to 2019) caused `backdrop-filter` to visually bleed through `opacity: 0` — the blur was rendered even when the element was invisible. This was resolved in Safari 13+ / WebKit 609+. The stated target is Safari 17.4+ (WebKit 17.4), which is well beyond the fix point. No bleed-through will occur at `opacity: 0`.

**Mitigation if needed (not required for stated targets)**: Setting `visibility: hidden` in the `from` keyframe alongside `opacity: 0` would hard-suppress `backdrop-filter` application. However, `visibility` is a discrete property — it cannot be smoothly interpolated — so it would cause an instant snap rather than a fade. Not used.

**Alternatives considered**:
- _`blur(0px)` in `from` keyframe_: Re-introduces animated `backdrop-filter`, which is the root cause of the original bug. Rejected.
- _`visibility: hidden` in keyframe_: Causes discrete snap, not a smooth fade. Rejected.

---

## R-3: `z-index: -1` on `::before` + `backdrop-filter` Sampling Behavior

**Decision**: Use `position: absolute; inset: 0; z-index: -1` on `.header::before`.

**Rationale**:
1. `position: sticky` creates a containing block for `position: absolute` descendants (CSS Positioned Layout spec, §9.7). `inset: 0` sizes `::before` to exactly match `.header`'s bounding box.
2. `.header` creates a stacking context (position: sticky + z-index: 1000). Within this stacking context, `z-index: -1` on `::before` renders it behind the nav's interactive content (links, hamburger, brand text) but inside `.header`'s own paint layer.
3. `backdrop-filter` on `::before` samples from the composited layers **behind `.header`'s stacking context** in the document — i.e., the scrolling page content beneath the header. This produces the correct glass blur effect.
4. `.header` has no `background` of its own after the fix (removed from `.header`, moved to `::before`), so no background overlap issue exists.

**Alternatives considered**:
- _`z-index: 0` on `::before`_: Would render on the same level as nav content, potentially obscuring hamburger / links. Rejected.
- _Negative margin / transform approach_: Overcomplicated. Rejected.

---

## R-4: `pointer-events` on `::before`

**Decision**: No explicit `pointer-events` declaration on `.header::before` is needed.

**Rationale**: `.header { pointer-events: none }` is already set. `.header * { pointer-events: auto }` re-enables pointer events on real DOM children but does **not** match `::before` pseudo-elements (CSS `*` universal selector does not select pseudo-elements). Therefore `::before` inherits `pointer-events: none` from `.header` via the cascade, and will not intercept touch/click events.

---

## Summary Table

| Question | Decision | Status |
|---|---|---|
| Scroll timeline on `::before` in Safari 17.4+ | Supported per spec; graceful degradation is acceptable | ✅ Resolved |
| `opacity: 0` + `backdrop-filter` bleed-through | Not an issue in Safari 17.4+ target | ✅ Resolved |
| `z-index: -1` + `backdrop-filter` sampling | Correct behavior; `::before` behind nav content, blur samples page | ✅ Resolved |
| `pointer-events` on `::before` | Inherited `none` from `.header`; no action needed | ✅ Resolved |
