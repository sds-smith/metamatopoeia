# Research: 004 Nav Scroll-Glass & Mobile Hamburger Menu

**Phase**: 0 — Outline & Research  
**Generated**: 2026-06-07

---

## R-1: CSS Scroll-Driven Animations

**Decision**: Use `animation-timeline: scroll(root)` with `animation-range: 0px 80px` and `animation-fill-mode: both`.

**Rationale**: CSS Scroll-Driven Animations (CSSWG Level 1 spec, shipping in Chrome 115+/Firefox 110+/Safari 17.4+) allow animating CSS properties based on scroll position without any JavaScript. The `scroll()` function with `root` targets the document root scroll container. `animation-range` constrains the playback to a precise pixel window. `animation-fill-mode: both` ensures the final (`to`) state persists when scroll exceeds 80px, and the initial (`from`) state applies at scroll = 0.

**Alternatives considered**:

- _JS `scroll` event listener_: Would consume 4 of the 4 remaining JS budget lines. Rejected in favour of zero-JS CSS path.
- _CSS `:root:has(...)` + scroll snap tricks_: Fragile, non-standard, and cannot detect arbitrary scroll position. Rejected.
- _`IntersectionObserver`_: Cleaner than scroll events but still JS. Rejected to preserve JS budget.

**Browser support**:

- Chrome/Edge 115+ ✅
- Firefox 110+ ✅
- Safari 17.4+ ✅
- Older browsers: animation property is silently ignored → nav remains permanently transparent. No broken layout.

---

## R-2: `backdrop-filter` Animation in `@keyframes`

**Decision**: Animate from `blur(0px)` (not `none`) to `blur(var(--lg-glass-blur))` in the `from`/`to` keyframes.

**Rationale**: CSS requires both keyframe values to be of the same type for interpolation. `none` and `blur(20px)` are not interpolatable in all browsers. `blur(0px)` and `blur(20px)` are numerically interpolatable and produce a smooth transition. Always include both `backdrop-filter` and `-webkit-backdrop-filter` in keyframes for Safari compatibility.

**Alternatives considered**:

- _`backdrop-filter: none` → `blur(20px)`_: Not guaranteed to interpolate. Rejected.
- _CSS `@property` for backdrop-filter_: Overcomplicated for this use case. Rejected.

---

## R-3: CSS-Only Hamburger Menu — Checkbox Pattern

**Decision**: A visually hidden but keyboard-focusable `<input type="checkbox" aria-label="Toggle navigation menu">` as first child of `<body>`, with a visible `<label>` inside `.nav`. CSS sibling selector chain: `.nav-menu-checkbox:checked ~ .header .nav-links`.

**Rationale**: The CSS general sibling combinator (`~`) selects subsequent siblings. Placing the checkbox before `<header>` makes `.header` a subsequent sibling. From there, descendant selectors reach `.nav-links` and `.hamburger-line` elements. This is the canonical CSS-only interactive toggle pattern, requiring zero JavaScript.

**Key structural rule**: The checkbox MUST be a DOM-earlier sibling of every element it controls via `~`. Placing it as the first child of `<body>` maximises reach.

**Accessibility**: The checkbox itself provides keyboard operation and the accessible name. The visible `.nav-hamburger` label provides pointer/touch activation but does not require `tabindex`. `.nav-menu-checkbox:focus-visible ~ .header .nav-hamburger` mirrors the checkbox focus state onto the visible control. `visibility: hidden` (not `display: none`) on the closed overlay ensures its links are removed from the accessibility tree and tab order while allowing CSS transitions.

**Alternatives considered**:

- _`<details>`/`<summary>` element_: Semantic and accessible, but the summary content cannot be styled as a hamburger icon reliably across browsers. Limited CSS control over the disclosure triangle. Rejected.
- _`:focus-within` on nav_: Cannot maintain open state after focus leaves. Rejected.

---

## R-4: `visibility` Transition Asymmetry

**Decision**: On close: `transition: ..., visibility 0ms var(--lg-glass-transition-duration)`. On open: `transition: ..., visibility 0ms` (no delay).

**Rationale**: `visibility` is not a numeric property and cannot be gradually interpolated. It snaps. The pattern "delay on close, instant on open" ensures:

- **Open**: `visibility: visible` fires immediately so the element is in the accessibility tree from frame 1.
- **Close**: `visibility: hidden` fires AFTER the opacity/transform transition completes (300ms), so the element doesn't disappear from the DOM while still visually animating out.

This is the standard pattern for accessible CSS-only show/hide transitions.

---

## R-5: Mobile-First Nav Layout

**Decision**: Make mobile hamburger behavior the base responsive style, then restore desktop behavior through `@media (min-width: 769px)`.

**Rationale**: The constitution requires responsive layouts to be mobile-first. With the hamburger menu, the base nav bar is a horizontal row (brand left, hamburger right), and `.nav-links` is a fixed hidden overlay by default. Desktop styles then hide the hamburger, hide the logo image, and restore `.nav-links` to a static horizontal row.

**Implementation note**: Remove the old desktop-first `@media (max-width: 768px)` nav/link overrides rather than appending conflicting rules. Add a desktop `@media (min-width: 769px)` block for desktop-only restoration.

---

## R-6: Z-Index Layering Strategy

**Decision**:

- `.header`: `z-index: 1000` (existing) — the sticky nav stacking context
- `.nav-links` overlay: local `z-index: 0`
- `.nav-brand` and `.nav-hamburger`: `position: relative; z-index: 1`

**Rationale**: The `.header` element creates a stacking context at z-index 1000. The `.nav-links` overlay is a child of `.header`; if it receives a high z-index, it can paint over the hamburger and block the close control. Keeping the overlay at local z-index 0 and raising `.nav-brand`/`.nav-hamburger` to local z-index 1 keeps the close control tappable while the header remains above page content.

---

## R-7: Hamburger Icon Morph Geometry

**Decision**: Three `<span class="hamburger-line">` elements in a flex column container (gap: 5px). Morph offsets: Line 1 `translateY(7px) rotate(45deg)`; Line 2 `opacity: 0; transform: scaleX(0)`; Line 3 `translateY(-7px) rotate(-45deg)`.

**Calculation**:

```
Container: 3 lines × 2px + 2 gaps × 5px = 16px total height
Container center: 8px from top
Line 1 center: 1px → move to 8px → translateY(+7px)
Line 3 center: 15px → move to 8px → translateY(-7px)
```

**Rationale**: All three lines must converge at the container center before rotating to form an X. Centering each line first, then rotating, produces a clean cross shape.

---

## Summary: All NEEDS CLARIFICATION Items Resolved

| Unknown                                  | Resolution                                                 |
| ---------------------------------------- | ---------------------------------------------------------- |
| Scroll transition implementation         | CSS Scroll-Driven Animations (R-1)                         |
| `backdrop-filter` keyframe interpolation | `blur(0px)` → `blur(Xpx)` (R-2)                            |
| Hamburger DOM placement for `~` selector | Checkbox as first child of `<body>` (R-3)                  |
| `visibility` timing on close             | Delayed `visibility: hidden` pattern (R-4)                 |
| Mobile nav flex conflict                 | Replace `column` with `row` in same media query (R-5)      |
| Z-index layering                         | Header at 1000, overlay at 999 within header context (R-6) |
| Hamburger morph offsets                  | `±7px` translateY, computed geometrically (R-7)            |
