# Feature Specification: Nav Scroll-Glass & Mobile Hamburger Menu

**Feature Branch**: `004-nav-scroll-glass-mobile-menu`
**Created**: 2026-06-07
**Status**: Clarified

---

## 1. Overview

Enhance the existing top navigation (`<header class="header">` / `.nav`) with three distinct behaviors:

1. **Scroll-aware surface transition** — nav is visually transparent at scroll top; transitions to a liquid glass surface as the user scrolls; returns to transparent when scroll returns to zero.
2. **CSS-only mobile hamburger menu** — on mobile viewports, nav links are hidden behind a full-screen slide-in overlay toggled by a hidden checkbox. Zero JavaScript.
3. **Nav-brand logo mobile-only** — the `<img class="nav-logo">` element is hidden on desktop and visible only on mobile viewports.

---

## 2. Functional Requirements

### FR-1: Scroll-Aware Nav Surface

| ID | Requirement |
|----|-------------|
| FR-1.1 | At scroll position 0, `.header` MUST be fully transparent (no background, no blur, no border, no shadow). |
| FR-1.2 | As the user scrolls, `.header` MUST transition to a liquid glass surface (see token spec in §4). |
| FR-1.3 | The transition MUST be driven entirely by CSS Scroll-Driven Animations (`animation-timeline: scroll(root)`). Zero JavaScript. |
| FR-1.4 | The glass surface MUST be fully applied by the time scroll position reaches **80px**. |
| FR-1.5 | Scrolling back to top MUST reverse the transition — returning `.header` to fully transparent. |
| FR-1.6 | Under `prefers-reduced-motion: reduce`, the scroll animation MUST be disabled. The nav MUST fall back to a persistent, static glass surface (no animated transition). |

### FR-2: CSS-Only Mobile Hamburger Menu

| ID | Requirement |
|----|-------------|
| FR-2.1 | A hidden `<input type="checkbox" id="nav-menu-toggle" class="nav-menu-checkbox">` MUST be placed as the **first child of `<body>`**, before `<header>`. |
| FR-2.2 | A `<label for="nav-menu-toggle" class="nav-hamburger">` MUST be placed inside `.nav`, between `.nav-brand` and `.nav-links`. |
| FR-2.3 | The hamburger label MUST contain exactly three `<span class="hamburger-line" aria-hidden="true">` children (the CSS-drawn icon lines). |
| FR-2.4 | The hamburger label MUST carry `aria-label="Toggle navigation menu"` and `tabindex="0"`. |
| FR-2.5 | On desktop (viewport width > 768px): hamburger label MUST be `display: none`. Nav links MUST always be visible. |
| FR-2.6 | On mobile (viewport width ≤ 768px): hamburger label MUST be visible. Nav links MUST be hidden by default. |
| FR-2.7 | When the checkbox is checked (menu open), `.nav-links` MUST slide in from the top as a full-screen fixed overlay. |
| FR-2.8 | The overlay MUST use a liquid glass surface (dark tinted background + `backdrop-filter: blur`) so underlying content is obscured but visible. |
| FR-2.9 | Nav links within the open overlay MUST be displayed as a centered vertical list with generous touch targets (min-height 48px per link). |
| FR-2.10 | When the checkbox is checked, the hamburger icon MUST morph to an ✕ (X): top line rotates +45°, middle line fades and scales out, bottom line rotates −45°. The morph MUST be a CSS transition, not an animation. |
| FR-2.11 | The `.header` (hamburger button row) MUST remain visually on top of the overlay (z-index layering: `.header` at 1000, overlay at 999). |
| FR-2.12 | **Accepted limitation**: The mobile overlay will NOT auto-close when an anchor link is clicked. The user must tap the ✕ to close. This is an accepted CSS-only constraint with zero JS. |
| FR-2.13 | Under `prefers-reduced-motion: reduce`, the slide-in transition MUST be instantaneous (no transform animation). |

### FR-3: Nav-Brand Logo Mobile-Only

| ID | Requirement |
|----|-------------|
| FR-3.1 | `<img class="nav-logo">` MUST be `display: none` by default (desktop). |
| FR-3.2 | `<img class="nav-logo">` MUST be `display: block` within the `@media (max-width: 768px)` breakpoint. |
| FR-3.3 | The `.nav-brand` text node "Metamatopoeia" MUST remain visible on both desktop and mobile. |

---

## 3. Non-Functional Requirements

| ID | Requirement |
|----|-------------|
| NFR-1 | Zero new JavaScript lines. The existing `<script>` block (speed-dial, 26 non-empty lines) is not modified. |
| NFR-2 | All new CSS MUST be added to `index.css` only. No new files. |
| NFR-3 | All new HTML MUST be added to `index.html` only. No new files. |
| NFR-4 | Browser support target: Chrome 115+, Firefox 110+, Safari 17.4+. Graceful degradation for older browsers (transparent nav, no broken layout). |
| NFR-5 | The mobile nav overlay links MUST be keyboard-navigable (tab through links; `visibility: hidden` when closed removes them from tab order). |
| NFR-6 | No new color values may be introduced. All colors MUST derive from the existing four palette tokens. |

---

## 4. Design Token Specification

### 4.1 Scroll-Activated Glass State (`.header` on scroll)

All tokens below are existing tokens in `index.css`. No new tokens need to be introduced for the scrolled glass state:

| Property | Value | Token Source |
|----------|-------|--------------|
| `background` | `var(--lg-glass-surface)` | Existing — `rgb(var(--color-frost-rgb) / 0.12)` |
| `backdrop-filter` | `blur(var(--lg-glass-blur))` | Existing — `20px` |
| `-webkit-backdrop-filter` | `blur(var(--lg-glass-blur))` | Existing |
| `box-shadow` | `var(--lg-glass-shadow-md)` | Existing |
| `border-bottom` | `1px solid var(--lg-glass-border)` | Existing |

### 4.2 New CSS Custom Properties Required

One new token group is needed for the nav scroll animation:

```css
/* Nav Scroll Animation */
--lg-nav-scroll-range: 80px;          /* scroll distance to reach full glass state */
--lg-nav-overlay-bg-opacity: 0.88;    /* mobile overlay dark background opacity */
```

Token naming follows `--lg-{group}-{subgroup}-{token}` convention (Principle II ✅).

### 4.3 Mobile Overlay Background

The mobile menu overlay uses a dark, high-opacity glass surface distinct from the card glass:

| Property | Value |
|----------|-------|
| `background` | `rgb(var(--color-slate-rgb) / var(--lg-nav-overlay-bg-opacity))` |
| `backdrop-filter` | `blur(var(--lg-glass-blur))` |

---

## 5. CSS Architecture

### 5.1 Scroll Animation (`@keyframes`)

```
@keyframes nav-glass-activate {
  from → transparent state (no background, no blur, no shadow, no border)
  to   → full glass state (see §4.1)
}

.header {
  animation-name: nav-glass-activate;
  animation-timing-function: linear;
  animation-fill-mode: both;
  animation-timeline: scroll(root);
  animation-range: 0px var(--lg-nav-scroll-range);
}
```

### 5.2 Hamburger to X Morph (CSS Transitions)

Hamburger lines use `transition: transform, opacity` driven by the checkbox sibling selector chain:

```
.nav-menu-checkbox:checked ~ .header .hamburger-line:nth-child(1) → translateY(+Xpx) rotate(45deg)
.nav-menu-checkbox:checked ~ .header .hamburger-line:nth-child(2) → opacity: 0, scaleX(0)
.nav-menu-checkbox:checked ~ .header .hamburger-line:nth-child(3) → translateY(-Xpx) rotate(-45deg)
```

The exact `translateY` offset (`X`) is computed from the hamburger container's line height + gap dimensions and MUST be resolved during implementation.

### 5.3 Mobile Overlay Selector Chain

```
/* Overlay open */
.nav-menu-checkbox:checked ~ .header .nav-links { transform: translateY(0); visibility: visible; opacity: 1; }

/* Desktop: override — always show links normally */
@media (min-width: 769px) {
  .nav-links { transform: none !important; visibility: visible !important; opacity: 1 !important; }
}
```

---

## 6. HTML Structural Changes

### 6.1 New Checkbox Element

Insert as **first child of `<body>`**:
```html
<input
  type="checkbox"
  id="nav-menu-toggle"
  class="nav-menu-checkbox"
  aria-hidden="true"
/>
```

### 6.2 Updated `.nav` Structure

```html
<nav class="nav">
  <div class="nav-brand">
    <img … class="nav-logo" />   <!-- hidden on desktop via CSS -->
    Metamatopoeia
  </div>
  <label                          <!-- NEW -->
    for="nav-menu-toggle"
    class="nav-hamburger"
    aria-label="Toggle navigation menu"
    tabindex="0"
  >
    <span class="hamburger-line" aria-hidden="true"></span>
    <span class="hamburger-line" aria-hidden="true"></span>
    <span class="hamburger-line" aria-hidden="true"></span>
  </label>
  <ul class="nav-links">          <!-- existing, behavior extended -->
    …
  </ul>
</nav>
```

---

## 7. Sections Affected in `index.css`

| Section | Change Type |
|---------|-------------|
| CSS Custom Properties (Root) | ADD 2 new tokens (`--lg-nav-scroll-range`, `--lg-nav-overlay-bg-opacity`) |
| HEADER & NAVIGATION | ADD scroll animation properties to `.header`; ADD `.nav-hamburger` and `.hamburger-line` rules; ADD `.nav-menu-checkbox` visibility rule; MODIFY `.nav-logo` to default `display: none` |
| New `@keyframes` block | ADD `nav-glass-activate` keyframe |
| New Sibling Selector block | ADD all `.nav-menu-checkbox:checked ~ ...` rules |
| MEDIA QUERIES — Mobile (max-width: 768px) | MODIFY `.nav` (remove `flex-direction: column`; set `flex-direction: row; justify-content: space-between; align-items: center`); ADD `.nav-logo { display: block }`; ADD mobile `.nav-links` overlay styles; ADD `.nav-hamburger { display: flex }` |
| MEDIA QUERIES — Reduced Motion | ADD `animation: none` override for `.header`; ADD static glass fallback for `.header` |

---

## 8. Out of Scope

- Auto-close mobile overlay on anchor link click (accepted CSS-only limitation, FR-2.12).
- Any JavaScript for the scroll or hamburger behaviors.
- Dark mode variation of the overlay (dark mode already uses the same palette tokens via existing `prefers-color-scheme` overrides — no additional work needed).
- Changes to the speed-dial FAB or any other component.
