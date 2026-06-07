# Context Snapshot: 004-nav-scroll-glass-mobile-menu

**Generated**: 2026-06-07
**Phases captured**: 2 (Discovery) + 3 (Specify) + 4 (Clarify)
**Purpose**: Restore full context for a cold-slate Phase 5 session (SWE-1.6). Read this before running /speckit.plan.

---

## 1. Active Spec

`spec.md` in `.specify/memory/` is the authoritative source. Feature branch: `004-nav-scroll-glass-mobile-menu`.

---

## 2. Codebase Snapshot (files relevant to this feature only)

| File | Lines | Relevance |
|------|-------|-----------|
| `index.html` | 468 | All HTML changes land here |
| `index.css` | 740 | All CSS changes land here |

### Current nav HTML structure (lines 30–77 of `index.html`)

```html
<header class="header">
  <nav class="nav">
    <div class="nav-brand">
      <img src="./assets/Metamatopoeia_simple_small.png" alt="Metamatopoeia" class="nav-logo" />
      Metamatopoeia
    </div>
    <ul class="nav-links">
      <li><a href="#hero" class="nav-link" …>home</a></li>
      <li><a href="#workshop" class="nav-link" …>workshop</a></li>
      <li><a href="https://sds-smith.io/about" class="nav-link" …>meet the founder</a></li>
      <li><a href="#contact" class="nav-link" …>contact</a></li>
    </ul>
  </nav>
</header>
```

### Current nav CSS (lines 295–343 of `index.css`)

```css
.header {
  position: sticky; top: 0; z-index: 1000;
  padding: 1rem 2rem; pointer-events: none;
}
.header * { pointer-events: auto; }
.nav { display: flex; justify-content: space-between; align-items: center; max-width: 1200px; margin: 0 auto; }
.nav-brand { font-size: var(--lg-typography-size-xl); font-weight: 700; color: var(--lg-color-text-primary); }
.nav-logo { display: block; height: 36px; width: auto; }
.nav-links { display: flex; gap: 2rem; list-style: none; }
.nav-link { color: var(--lg-color-text-primary); font-weight: 500; transition: … }
```

### Current mobile nav CSS (lines 700–710 of `index.css`)

```css
@media (max-width: 768px) {
  .nav { flex-direction: column; gap: 1rem; }        ← MUST be replaced with row layout
  .nav-links { flex-wrap: wrap; justify-content: center; gap: 1rem; }  ← MUST be fully replaced
}
```

### Existing JS block (lines 438–465 of `index.html`)
26 non-empty lines. Manages the speed-dial FAB. **DO NOT MODIFY.** JS budget: 4 lines remain.

---

## 3. Resolved Implementation Details

### 3.1 Hamburger Icon Dimensions and Morph Offsets

- Each `.hamburger-line`: `width: 100%`, `height: 2px`, `border-radius: 2px`
- Flex container (`.nav-hamburger`): `flex-direction: column; gap: 5px`
- Container total height: 3×2px + 2×5px = **16px**
- Container center: **8px** from top
- **Line 1 center** = 1px → `translateY(+7px) rotate(45deg)` when open
- **Line 2** → `opacity: 0; transform: scaleX(0)` when open
- **Line 3 center** = 15px → `translateY(-7px) rotate(-45deg)` when open
- Transition duration: `var(--lg-glass-transition-duration)` (300ms; collapses to 0ms on `prefers-reduced-motion`)

### 3.2 Scroll Animation Keyframe

```css
@keyframes nav-glass-activate {
  from {
    background: transparent;
    backdrop-filter: blur(0px);
    -webkit-backdrop-filter: blur(0px);
    box-shadow: none;
    border-bottom: 1px solid transparent;
  }
  to {
    background: var(--lg-glass-surface);
    backdrop-filter: blur(var(--lg-glass-blur));
    -webkit-backdrop-filter: blur(var(--lg-glass-blur));
    box-shadow: var(--lg-glass-shadow-md);
    border-bottom: 1px solid var(--lg-glass-border);
  }
}

.header {
  /* ADD these — do not remove existing rules */
  animation-name: nav-glass-activate;
  animation-timing-function: linear;
  animation-fill-mode: both;
  animation-timeline: scroll(root);
  animation-range: 0px var(--lg-nav-scroll-range);
}
```

**Why `animation-fill-mode: both`?** At scroll > 80px the animation progress exceeds 100% — `both` ensures the `to` (glass) state persists. At scroll = 0 the progress is exactly 0% → `from` state applies naturally without fill.

### 3.3 Mobile Overlay Selector Chain (inside `@media (max-width: 768px)` only)

```css
/* Closed state (default on mobile) */
.nav-links {
  position: fixed; top: 0; left: 0; right: 0; bottom: 0;
  background: rgb(var(--color-slate-rgb) / var(--lg-nav-overlay-bg-opacity));
  backdrop-filter: blur(var(--lg-glass-blur));
  -webkit-backdrop-filter: blur(var(--lg-glass-blur));
  flex-direction: column; align-items: center; justify-content: center;
  gap: 2rem; list-style: none;
  transform: translateY(-100%);
  visibility: hidden;
  opacity: 0;
  z-index: 999;
  transition:
    transform var(--lg-glass-transition-duration) var(--lg-glass-transition-easing),
    opacity var(--lg-glass-transition-duration) var(--lg-glass-transition-easing),
    visibility 0ms var(--lg-glass-transition-duration);
}

/* Open state */
.nav-menu-checkbox:checked ~ .header .nav-links {
  transform: translateY(0);
  visibility: visible;
  opacity: 1;
  transition:
    transform var(--lg-glass-transition-duration) var(--lg-glass-transition-easing),
    opacity var(--lg-glass-transition-duration) var(--lg-glass-transition-easing),
    visibility 0ms;
}
```

**Important**: `visibility` transition asymmetry — delay on close (waits for opacity to finish), instant on open. This prevents tab-accessible links from being reachable while the menu is visually closed.

**No `!important` needed.** Overlay styles live only inside `@media (max-width: 768px)`. Desktop always sees the default `.nav-links` flex row from outside the media query.

### 3.4 Mobile Nav Link Styles in Overlay

```css
@media (max-width: 768px) {
  .nav-links .nav-link {
    font-size: var(--lg-typography-size-lg);
    min-height: 48px;
    display: flex; align-items: center;
    padding: 0 1.5rem;
    color: var(--lg-color-text-primary);
  }
}
```

### 3.5 New CSS Custom Properties

```css
/* Nav Scroll Animation */
--lg-nav-scroll-range: 80px;
--lg-nav-overlay-bg-opacity: 0.88;
```

---

## 4. Constitutional Compliance Decisions

| Principle | Decision |
|-----------|----------|
| **I (Zero JS)** | Zero new JS lines consumed. Hamburger: CSS checkbox. Scroll: CSS Scroll-Driven Animations. |
| **II (Liquid Glass CSS)** | Two new tokens added following `--lg-{group}-{subgroup}-{token}` convention. All animation via CSS. |
| **III (Palette)** | No new color hex values. All colors derive from existing `--color-*-rgb` tokens. |
| **IV (Three-section nav)** | Nav link structure (home/workshop/meet the founder/contact) unchanged. |
| **V (A11y)** | `tabindex="0"` + `aria-label` on hamburger `<label>`. `visibility: hidden` removes closed links from tab order. Touch targets ≥ 48px. |

---

## 5. Accepted Limitations (documented in spec §8)

- **No auto-close on link click** — CSS-only constraint. User taps ✕ to close. Acceptable for the portfolio use case.
- **Browser support** — Chrome 115+, Firefox 110+, Safari 17.4+ for scroll-driven animations. Older browsers: transparent nav with no broken layout.
- **Label keyboard activation** — `<label tabindex="0">` keyboard Space activation works in modern browsers. No JS fallback for older browsers.

---

## 6. Files Changed (summary for planner)

| File | Change Type | Sections Affected |
|------|-------------|-------------------|
| `index.html` | ADD checkbox before `<header>`; ADD hamburger `<label>` inside `.nav` | Lines 29–77 region |
| `index.css` | ADD 2 tokens; ADD `@keyframes`; MODIFY `.header`; ADD hamburger rules; MODIFY mobile nav rules | Multiple sections |

---

## 7. Explicit Non-Goals

- Do NOT modify the speed-dial FAB or its JS.
- Do NOT create new files.
- Do NOT add JS for any of these three features.
- Do NOT change link text, hrefs, or ARIA labels of existing nav links.
