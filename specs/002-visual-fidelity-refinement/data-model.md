# Data Model: 002 Layout Refinement — Metamatopoeia Visual Fidelity

**Phase**: Phase 1 | **Date**: 2026-06-06  
**Note**: This is a CSS class system model — not a data persistence model. The "entities" are CSS classes, design tokens, and HTML element relationships.

---

## CSS Class Inventory

### Classes Removed (`index.css` + `index.html`)

| Class | File | Replaced By |
| --- | --- | --- |
| `.project-card` | CSS + HTML | Removed from HTML; CSS rule deleted |
| `.project-image` | CSS + HTML | `.media` |
| `.project-content` | CSS + HTML | `.content` |
| `.project-actions` | CSS + HTML | `.card-actions` |
| `.project-description` | CSS + HTML | `.portfolio-description` |
| `.project-grid` | CSS + HTML | `.portfolio-links` |

### Classes Added (`index.css`)

| Class | Key Declarations | Affected Elements |
| --- | --- | --- |
| `.media` | `display: block; width: 100%; object-fit: cover; background-size: cover; background-repeat: no-repeat; background-position: center;` | Workshop card `<img>` elements |
| `.content` | `position: relative; z-index: 1; padding: 16px;` | Workshop card content `<div>` |
| `.card-actions` | `display: flex; gap: 8px; margin-top: 16px; flex-wrap: wrap;` | Workshop card action `<div>` |
| `.portfolio-links` | `display: flex; flex-direction: column; gap: 24px;` | Workshop section grid `<div>` |
| `.layout-list .card` | `width: 100%;` | All `.card` descendants of `.layout-list` sections |
| `.header *` | `pointer-events: auto;` | All children of `<header>` |

### Classes Modified (`index.css`)

| Class | What Changes | Before | After |
| --- | --- | --- | --- |
| `.card-elevated` | Remove padding | `padding: 2rem` | *(removed)* |
| `.card-elevated` | Update border-radius | `border-radius: 16px` (or existing value) | `border-radius: 24px` |
| `.card-elevated` | Add overflow | *(absent)* | `overflow: hidden` |
| `.card-elevated::before` → `::after` | Change pseudo-element + inset | `height: 2px; top: 0; opacity: 0.6` | `inset: 0; border-radius: inherit; pointer-events: none` |
| `.card::before` → `::after` | Change pseudo-element + inset | `height: 1px; top: 0; opacity: 0.5` | `inset: 0; border-radius: inherit; pointer-events: none` |
| `.header` | Remove glass surface | `background`, `backdrop-filter`, `-webkit-backdrop-filter`, `border-bottom` | *(all removed)* |
| `.header` | Add pointer-events | *(absent)* | `pointer-events: none` |
| `.hero-card` | Add padding | *(inherited from card-elevated)* | `padding: 2rem` |
| `.contact-card` | Add padding | *(inherited from card-elevated)* | `padding: 2rem` |
| `.button` | Update to glass-surface style | No background/border (depended on modifier) | `border: 1px solid var(--lg-glass-border); background: var(--lg-glass-surface); color: var(--lg-color-text-muted); padding: 8px 16px;` |

### Pseudo-element Migration

Both `.card::before` and `.card-elevated::before` migrate to `::after` for full-card inset glass reflection:

```
Before: ::before { position: absolute; top: 0; left: 0; right: 0; height: 1-2px; }
After:  ::after  { position: absolute; inset: 0; border-radius: inherit; pointer-events: none; }
```

---

## Design Token Inventory

### Tokens Modified (`:root`)

| Token | Before | After | Rationale |
| --- | --- | --- | --- |
| `--lg-color-text-primary` | `var(--color-slate)` | `var(--color-frost)` | Light text on dark-overlaid background |
| `--lg-glass-reflection` | 2-stop `--color-frost-rgb` gradient | 4-stop `--color-slate-rgb` gradient | Slate-toned reflection for dark composition |

### Token Values Confirmed Unchanged

These existing tokens are referenced by the new rules — their values are not changed:

| Token | Current Value | Used By |
| --- | --- | --- |
| `--lg-page-background-overlay` | `rgb(var(--color-slate-rgb) / 0.85)` | `body::before` overlay layer |
| `--lg-glass-border` | `rgb(var(--color-mist-rgb) / 0.25)` | `.button` border |
| `--lg-glass-surface` | `rgb(var(--color-frost-rgb) / var(--lg-glass-bg-opacity))` | `.button` background |
| `--lg-color-text-muted` | `var(--color-mist)` | `.button` text color |
| `--lg-typography-size-xs` | `clamp(...)` | `.button` font-size |
| `--color-slate-rgb` | `90 96 106` | `--lg-glass-reflection` stops |

---

## HTML Element Mapping (Workshop Section)

### Before

```html
<div class="project-grid">
  <article class="card card-elevated project-card">
    <img class="project-image" ... />
    <div class="project-content">
      <h3 class="project-title">...</h3>
      <p class="project-description">...</p>
      <div class="project-actions">
        <a class="button button-secondary" href="...">...</a>
      </div>
    </div>
  </article>
</div>
```

### After

```html
<div class="portfolio-links">
  <article class="card card-elevated">
    <img class="media" ... />
    <div class="content">
      <h3 class="project-title">...</h3>
      <p class="portfolio-description">...</p>
      <div class="card-actions">
        <a class="button" href="...">...</a>
      </div>
    </div>
  </article>
</div>
```

---

## `body::before` Pseudo-element (New Entity)

| Property | Value |
| --- | --- |
| `content` | `""` |
| `position` | `fixed` |
| `top / left / right / bottom` | `0` |
| `z-index` | `-1` |
| `background` (layer 1 — overlay) | `linear-gradient(var(--lg-page-background-overlay), var(--lg-page-background-overlay))` |
| `background` (layer 2 — image) | `url("./assets/background-image-profile.jpeg")` |
| `background` (layer 3 — fallback) | `linear-gradient(135deg, var(--color-slate) 0%, var(--color-teal) 100%)` |
| `background-size` | `auto 100vh` (desktop); `cover` (≤768px) |
| `background-position` | `right` |
| `background-repeat` | `no-repeat` |
| `pointer-events` | `none` |

---

## FAB Icon Entity

| Property | Before | After |
| --- | --- | --- |
| SVG element | `<line x1="12" y1="5" x2="12" y2="19"><line x1="5" y1="12" x2="19" y2="12">` | `<line x1="22" y1="2" x2="11" y2="13"><polygon points="22 2 15 22 11 13 2 9 22 2">` |
| Icon metaphor | "+" (add/close) | Paper plane (send/contact) |
| Open-state transform | `rotate(45deg)` | *(none — CSS rule removed)* |

---

## Validation Rules

1. All CSS color values must trace to `--color-slate` (`#5A606A`), `--color-teal` (`#79A1A2`), `--color-mist` (`#BDBFC6`), or `--color-frost` (`#EFF1F3`)
2. `body::before` overlay MUST use `var(--lg-page-background-overlay)` — not literal `rgba(0,0,0,...)`
3. `body::before` fallback gradient MUST use `var(--color-slate)` and `var(--color-teal)` — no raw hex
4. `--lg-glass-reflection` stops MUST reference `rgb(var(--color-slate-rgb) / ...)` — not frost-rgb
5. No new `<script>` content; existing 7-line IIFE unchanged
6. All `<img>` elements retain `loading="lazy"` and `alt` attributes
