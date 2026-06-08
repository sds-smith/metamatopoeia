# Bug Fix Specification: Nav Glass Scroll Transition — Mobile Blur Incomplete

**Bug ID**: `005-nav-glass-blur-mobile`
**Created**: 2026-06-08
**Status**: Clarified

---

## 1. Overview

### 1.1 Bug Description

On iOS Chrome (WebKit engine), the `backdrop-filter` blur on the nav scroll-glass transition is intermittently absent. The semi-transparent background tint (`background: var(--lg-glass-surface)`) applies correctly, but the `backdrop-filter: blur()` fails to composite. The failure is non-deterministic — it occurs randomly on the same device, same page load. Main content remains legible under the navigation header, which is visually confusing.

### 1.2 Root Cause

The current implementation animates `backdrop-filter` directly inside `@keyframes nav-glass-activate` driven by `animation-timeline: scroll(root)` on the `position: sticky` `.header` element. WebKit's GPU compositor does not reliably promote animated `backdrop-filter` to a composited layer when driven by a CSS Scroll-Driven Animation. The compositor thread receives the scroll position update but fails to apply the `backdrop-filter` value intermittently, producing a non-blurred glass surface.

The `background` property interpolates correctly because it is resolved on the main thread. `backdrop-filter` requires GPU compositing and is therefore susceptible to this compositor timing race on WebKit.

### 1.3 Fix Strategy

Replace the animated `backdrop-filter` approach with a `::before` pseudo-element approach:

- `.header::before` carries the **static** (non-animated) glass surface properties: `backdrop-filter`, `background`, `box-shadow`, `border-bottom`.
- The Scroll-Driven Animation is moved to `.header::before` and animates only **`opacity` (0 → 1)**.
- `opacity` animation is universally reliable on GPU compositors, including WebKit.
- The `@keyframes nav-glass-activate` block is replaced with a simpler `@keyframes nav-glass-surface-reveal` (opacity only).
- `.header` itself loses all scroll animation properties.

---

## 2. Functional Requirements

| ID     | Requirement                                                                                                                                                                                                                        |
| ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR-1.1 | At scroll position 0, the nav glass surface MUST be fully invisible. `opacity: 0` on `.header::before` achieves this.                                                                                                              |
| FR-1.2 | As the user scrolls, the glass surface MUST transition to fully opaque by the time scroll position reaches `--lg-nav-scroll-range` (80px). This is achieved by the scroll-driven opacity animation on `.header::before`.           |
| FR-1.3 | The `backdrop-filter` blur MUST be applied reliably on iOS Chrome (WebKit). Achieved by keeping `backdrop-filter` static on `::before` (not animated).                                                                            |
| FR-1.4 | Scrolling back to top MUST reverse the transition — `::before` returns to `opacity: 0`.                                                                                                                                            |
| FR-1.5 | The `.header::before` MUST NOT intercept pointer events. It inherits `pointer-events: none` from `.header` and is not matched by `.header * { pointer-events: auto }`, so no explicit override is required.                       |
| FR-1.6 | Under `prefers-reduced-motion: reduce`, the scroll animation on `.header::before` MUST be disabled. `.header::before` MUST be set to `animation: none; opacity: 1` so the static glass is always visible. The `.header { animation: none }` override (and its inline glass properties) MUST be removed — glass state is now owned by `::before`. |
| FR-1.7 | Under `prefers-reduced-transparency: reduce`, `backdrop-filter: none` MUST be applied to `.header::before` (replacing the existing `.header` entry in that media block).                                                           |

---

## 3. Non-Functional Requirements

| ID    | Requirement                                                                                                                               |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| NFR-1 | Zero new JavaScript lines. The existing `<script>` block is not modified.                                                                 |
| NFR-2 | All CSS changes MUST be confined to `index.css`. No new files.                                                                            |
| NFR-3 | No new HTML changes.                                                                                                                      |
| NFR-4 | No new CSS custom properties are required. All existing tokens (`--lg-glass-blur`, `--lg-glass-surface`, `--lg-glass-shadow-md`, `--lg-glass-border`, `--lg-nav-scroll-range`) are reused. |
| NFR-5 | The visual result MUST be indistinguishable from the current working state on desktop browsers — only the implementation mechanism changes. |
| NFR-6 | No new color values. All colors derive from existing palette tokens.                                                                       |

---

## 4. CSS Architecture

### 4.1 New `@keyframes` Block

Replace `@keyframes nav-glass-activate` with:

```css
@keyframes nav-glass-surface-reveal {
  from { opacity: 0; }
  to   { opacity: 1; }
}
```

### 4.2 `.header` Changes

**Remove** the scroll animation properties from `.header`:

```
/* REMOVE from .header: */
animation-name: nav-glass-activate;
animation-timing-function: linear;
animation-fill-mode: both;
animation-timeline: scroll(root);
animation-range: 0px var(--lg-nav-scroll-range);
```

`.header` retains: `position: sticky; top: 0; z-index: 1000; padding: 1rem 2rem; pointer-events: none`.

### 4.3 New `.header::before` Rule

```css
.header::before {
  content: '';
  position: absolute;
  inset: 0;
  z-index: -1;
  backdrop-filter: blur(var(--lg-glass-blur));
  -webkit-backdrop-filter: blur(var(--lg-glass-blur));
  background: var(--lg-glass-surface);
  box-shadow: var(--lg-glass-shadow-md);
  border-bottom: 1px solid var(--lg-glass-border);
  animation-name: nav-glass-surface-reveal;
  animation-timing-function: linear;
  animation-fill-mode: both;
  animation-timeline: scroll(root);
  animation-range: 0px var(--lg-nav-scroll-range);
}
```

**Rationale for `z-index: -1`**: `.header` creates a stacking context (`position: sticky; z-index: 1000`). `z-index: -1` on `::before` renders it behind `.header`'s content (nav links, brand, hamburger) but in front of elements at lower z-index levels in the document, ensuring the blur correctly samples the scrolling page content beneath.

### 4.4 `prefers-reduced-motion` Block Changes

**Remove** the existing `.header` override in this block:

```css
/* REMOVE: */
.header {
  animation: none;
  background: var(--lg-glass-surface);
  backdrop-filter: blur(var(--lg-glass-blur));
  -webkit-backdrop-filter: blur(var(--lg-glass-blur));
  box-shadow: var(--lg-glass-shadow-md);
  border-bottom: 1px solid var(--lg-glass-border);
}
```

**Add** `.header::before` override:

```css
/* ADD: */
.header::before {
  animation: none;
  opacity: 1;
}
```

### 4.5 `prefers-reduced-transparency` Block Changes

Replace the `.header` entry in the `backdrop-filter: none` selector list:

```css
/* CHANGE: */
.card,
.card-elevated,
.header,      /* ← becomes .header::before */
.speed-dial-action {
  backdrop-filter: none;
  -webkit-backdrop-filter: none;
}
```

To:

```css
.card,
.card-elevated,
.header::before,
.speed-dial-action {
  backdrop-filter: none;
  -webkit-backdrop-filter: none;
}
```

---

## 5. Sections Affected in `index.css`

| Section                               | Change Type                                                                                                          |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| HEADER & NAVIGATION — `.header` rule  | REMOVE 5 scroll animation properties                                                                                 |
| NAV SCROLL ANIMATION block            | RENAME `@keyframes nav-glass-activate` → `nav-glass-surface-reveal`; replace body with opacity-only keyframe         |
| NAV SCROLL ANIMATION block            | ADD `.header::before` rule with static glass properties + opacity scroll-driven animation                            |
| `prefers-reduced-motion` block        | REMOVE `.header` override; ADD `.header::before { animation: none; opacity: 1; }`                                   |
| `prefers-reduced-transparency` block  | REPLACE `.header` with `.header::before` in the `backdrop-filter: none` selector list                               |

---

## 6. Out of Scope

- No changes to HTML structure.
- No changes to the mobile hamburger menu behavior or overlay.
- No changes to dark mode CSS (palette tokens are unchanged; `::before` inherits dark-mode token overrides automatically).
- No changes to the speed-dial, hero, workshop, or contact sections.
- No new JavaScript.
