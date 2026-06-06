# Research: 002 Layout Refinement — Metamatopoeia Visual Fidelity

**Phase**: Phase 0 | **Date**: 2026-06-06  
**Status**: Complete — zero open unknowns

## Overview

All technical unknowns were resolved during Phases 2–4 (Discovery → Definition → Clarify) by reading the live model site source at `sds-smith/html_portfolio` (`public/index.html`, `public/index.css`, `public/portfolio.html`). No external research beyond the model site is required. This document records every decision and its rationale.

---

## D-001: Background Image Rendering (FR-001 to FR-006)

**Decision**: Use `body::before` pseudo-element with `position: fixed; z-index: -1` carrying a 3-layer `background` shorthand.

**Rationale**: `body::before` decouples background rendering from the document flow. `position: fixed` pins it to the viewport regardless of scroll. `z-index: -1` places it behind all content stacking contexts. Using `background-size: auto 100vh; background-position: right; background-repeat: no-repeat` creates the right-justified partial-width treatment — the image renders at native aspect ratio scaled to viewport height, anchored to the right edge, leaving the left exposed for the gradient fallback.

**Overlay token decision**: The spec (FR-003) mandates `rgb(var(--color-slate-rgb) / 0.85)`. However, the existing `:root` already declares `--lg-page-background-overlay: rgb(var(--color-slate-rgb) / 0.85)`, and the `@media (prefers-color-scheme: dark)` block overrides it to `rgb(var(--color-slate-rgb) / 0.9)`. Using `var(--lg-page-background-overlay)` in `body::before` is strictly better — it derives from `--color-slate-rgb` (satisfying FR-003 and FR-026) and automatically respects dark mode. This is an advisory improvement over FR-003's literal suggestion.

**Final CSS**:
```css
body::before {
  content: "";
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: -1;
  background:
    linear-gradient(var(--lg-page-background-overlay), var(--lg-page-background-overlay)),
    url("./assets/background-image-profile.jpeg"),
    linear-gradient(135deg, var(--color-slate) 0%, var(--color-teal) 100%);
  background-size: auto 100vh;
  background-position: right;
  background-repeat: no-repeat;
  pointer-events: none;
}
```

**Mobile override** (in existing `@media (max-width: 768px)` block):
```css
body::before {
  background-size: cover;
}
```

**Alternatives rejected**:
- `body { background-attachment: fixed; background-size: cover; }` (current) — `cover` fills full width; cannot produce partial-width right-justified treatment
- `background-size: contain; background-position: right` on `body` — no pseudo-element means the gradient and image can't be separately layered without `body::before`

---

## D-002: Header Transparency (FR-007 to FR-010)

**Decision**: Remove `background`, `backdrop-filter`, `-webkit-backdrop-filter`, and `border-bottom` from `.header`. Add `pointer-events: none` and `.header *` `pointer-events: auto`.

**Rationale**: The model's header has `pointer-events: none` and no background surface, making it visually indistinguishable from the page backdrop. The `pointer-events` trick passes click-through to the transparent header area while keeping nav links and brand text interactive via the `*` override.

**Risk acknowledged**: Content scrolling behind the header is fully visible (no frosting buffer). This is the intentional effect per the model's design language.

---

## D-003: Workshop Card Structure (FR-011 to FR-020b)

**Decision**: Remap all workshop cards from `.project-*` custom classes to the model's `.media` / `.content` / `.card-actions` / `.portfolio-links` class system.

**Key values confirmed from model `portfolio.html` source**:

| Rule | Declarations |
| --- | --- |
| `.card-elevated` | `border-radius: 24px; overflow: hidden;` — **no padding** |
| `.card-elevated::after` | `content: ""; position: absolute; inset: 0; border-radius: inherit; background: var(--lg-glass-reflection); pointer-events: none;` |
| `.media` | `display: block; width: 100%; object-fit: cover;` |
| `.content` | `position: relative; z-index: 1; padding: 16px;` |
| `.card-actions` | `display: flex; gap: 8px; margin-top: 16px; flex-wrap: wrap;` |
| `.portfolio-links` | `display: flex; flex-direction: column; gap: 24px;` |

**Hero/Contact card padding recovery (FR-012a)**: Removing `padding: 2rem` from `.card-elevated` affects ALL elevated cards, including Hero and Contact. Resolution: add `padding: 2rem` explicitly to `.hero-card` and `.contact-card` CSS rules.

**Button resolution (FR-020a)**: Workshop cards change from `<a class="button button-secondary">` to `<a class="button">`. The base `.button` CSS is updated to a glass-surface style:
```css
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 8px 16px;
  border-radius: 8px;
  border: 1px solid var(--lg-glass-border);
  background: var(--lg-glass-surface);
  color: var(--lg-color-text-muted);
  font-size: var(--lg-typography-size-xs);
  cursor: pointer;
  transition: all var(--lg-glass-transition-duration) var(--lg-glass-transition-easing);
  text-decoration: none;
}
```
`.button-primary` override remains unchanged (teal-background Hero CTA).

**Full-width cards (FR-020b)**: Added `.layout-list .card { width: 100%; }` to guarantee full-width in flex-column layout. While flex `align-items: stretch` typically handles this, explicit is better in cross-browser context.

**Semantic decisions (Phase 2 intent locks)**:
- `<h3 class="project-title">` retained (not `<span>`) to preserve heading hierarchy
- `<a class="button">` used (not `<a><button>`) for HTML validity

---

## D-004: Color Token Reassignment (FR-021 to FR-023)

**Decision**: `--lg-color-text-primary: var(--color-frost)` in `:root`. `--lg-glass-reflection` uses 4-stop `--color-slate-rgb` gradient.

**Text color rationale**: The background has a heavy `--lg-page-background-overlay` (`0.85` opacity slate). Dark text (`--color-slate`, `#5A606A`) on a dark background creates poor contrast. Switching to `--color-frost` (`#EFF1F3`) resolves legibility.

**Reflection gradient rationale**: Using `--color-slate-rgb` instead of `--color-frost-rgb` produces a darker, subtler glass sheen against the dark-background composition. The model uses white (`rgba(255,255,255,...)`) for its black-background. Slate serves as our brand-palette equivalent.

**Final token values**:
```css
--lg-color-text-primary: var(--color-frost);

--lg-glass-reflection: linear-gradient(
  135deg,
  rgb(var(--color-slate-rgb) / 0.4) 0%,
  rgb(var(--color-slate-rgb) / 0) 40%,
  rgb(var(--color-slate-rgb) / 0) 60%,
  rgb(var(--color-slate-rgb) / 0.15) 100%
);
```

**Cascade effects**:
- Nav brand, nav links, section titles, hero title, hero tagline, project titles, descriptions, contact labels — all switch from slate (#5A606A) to frost (#EFF1F3)
- `.button-primary` (uses `--lg-color-accent-contrast: var(--color-frost)`) — unaffected
- `--lg-color-text-muted` (remains `var(--color-mist)`) — unaffected
- Dark mode block retains `--lg-color-text-primary: var(--color-frost)` override per Phase 2 Q3 intent lock

---

## D-005: FAB Paper Plane Icon (FR-024 to FR-025)

**Decision**: Replace the current `+` cross SVG with the Lucide `Send` (paper plane) icon. Remove the `rotate(45deg)` CSS rule.

**SVG path confirmed from live model site source**:
```html
<svg class="speed-dial-icon" aria-hidden="true" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <line x1="22" y1="2" x2="11" y2="13"></line>
  <polygon points="22 2 15 22 11 13 2 9 22 2"></polygon>
</svg>
```

**CSS rule to remove**:
```css
.speed-dial-checkbox:checked ~ .speed-dial-fab .speed-dial-icon {
  transform: rotate(45deg);
}
```

**Rationale**: Paper plane communicates "send/contact" metaphor. Static icon (no rotation on open) reduces animation noise and matches model behavior exactly.
