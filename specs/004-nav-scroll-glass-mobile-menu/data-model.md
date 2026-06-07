# Data Model: 004 Nav Scroll-Glass & Mobile Hamburger Menu

**Phase**: 1 — Design & Contracts  
**Generated**: 2026-06-07

> For a pure HTML/CSS feature, "data model" = CSS class inventory + token additions + HTML element inventory.

---

## 1. New CSS Custom Properties

| Token                         | Value  | Group | Purpose                                                     |
| ----------------------------- | ------ | ----- | ----------------------------------------------------------- |
| `--lg-nav-scroll-range`       | `80px` | nav   | Scroll distance at which `.header` reaches full glass state |
| `--lg-nav-overlay-bg-opacity` | `0.88` | nav   | Opacity of the mobile menu overlay background               |

**Placement**: `:root` block, "Nav Scroll Animation" sub-section, after existing transition tokens.  
**Naming compliance**: Both follow `--lg-{group}-{subgroup?}-{token}` (Principle II ✅).

---

## 2. New `@keyframes`

| Name                 | From state                                                                                          | To state                                                                                                                                                                   |
| -------------------- | --------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `nav-glass-activate` | `background: transparent; backdrop-filter: blur(0px); box-shadow: none; border-bottom: transparent` | `background: var(--lg-glass-surface); backdrop-filter: blur(var(--lg-glass-blur)); box-shadow: var(--lg-glass-shadow-md); border-bottom: 1px solid var(--lg-glass-border)` |

**Placement**: New `/* NAV SCROLL ANIMATION */` section in `index.css`, between existing HEADER & NAVIGATION and HERO SECTION sections.

---

## 3. Modified CSS Classes

### `.header` (index.css:295–301)

| Property                    | Before | After                            |
| --------------------------- | ------ | -------------------------------- |
| `animation-name`            | —      | `nav-glass-activate`             |
| `animation-timing-function` | —      | `linear`                         |
| `animation-fill-mode`       | —      | `both`                           |
| `animation-timeline`        | —      | `scroll(root)`                   |
| `animation-range`           | —      | `0px var(--lg-nav-scroll-range)` |

### `.nav-logo` (index.css:321–325)

| Property  | Before  | After  |
| --------- | ------- | ------ |
| `display` | `block` | `none` |

_(Mobile media query restores `display: block`.)_

### `.nav` — mobile override (index.css:701–704)

| Property          | Before   | After           |
| ----------------- | -------- | --------------- |
| `flex-direction`  | `column` | `row`           |
| `gap`             | `1rem`   | _(removed)_     |
| `justify-content` | —        | `space-between` |
| `align-items`     | —        | `center`        |

### `.nav-links` — mobile override (index.css:705–710)

Existing `flex-wrap: wrap; justify-content: center; gap: 1rem` rules replaced by full overlay styles (see New Classes below).

---

## 4. New CSS Classes

| Class                | Element                   | Purpose                                           |
| -------------------- | ------------------------- | ------------------------------------------------- |
| `.nav-menu-checkbox` | `<input type="checkbox">` | Hidden toggle control; visually removed from flow |
| `.nav-hamburger`     | `<label>`                 | Hamburger/X toggle button; mobile-only            |
| `.hamburger-line`    | `<span>` × 3              | Individual icon lines; CSS-drawn                  |

### `.nav-menu-checkbox`

```
position: absolute; opacity: 0; pointer-events: none; width: 0; height: 0
```

### `.nav-hamburger` (desktop default)

```
display: none
```

### `.nav-hamburger` (mobile override — max-width: 768px)

```
display: flex; flex-direction: column; gap: 5px;
width: 24px; cursor: pointer; padding: 4px;
position: relative; z-index: 1;
```

### `.hamburger-line`

```
width: 100%; height: 2px; background: var(--lg-color-text-primary);
border-radius: 2px;
transition: transform var(--lg-glass-transition-duration) var(--lg-glass-transition-easing),
            opacity var(--lg-glass-transition-duration) var(--lg-glass-transition-easing);
```

---

## 5. New Sibling Selector Rules

Overlay and morph rules apply to mobile-first base styles. Desktop `@media (min-width: 769px)` overrides keep nav links visible and hamburger hidden.

| Selector                                                            | Properties Set                                                                     |
| ------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| `.nav-menu-checkbox:checked ~ .header .nav-links`                   | `transform: translateY(0); visibility: visible; opacity: 1; transition (no delay)` |
| `.nav-menu-checkbox:checked ~ .header .hamburger-line:nth-child(1)` | `transform: translateY(7px) rotate(45deg)`                                         |
| `.nav-menu-checkbox:checked ~ .header .hamburger-line:nth-child(2)` | `opacity: 0; transform: scaleX(0)`                                                 |
| `.nav-menu-checkbox:checked ~ .header .hamburger-line:nth-child(3)` | `transform: translateY(-7px) rotate(-45deg)`                                       |

---

## 6. New HTML Elements

### `<input>` — nav-menu-checkbox

```
Location: first child of <body>, before <header class="header">
Attributes: type="checkbox" id="nav-menu-toggle" class="nav-menu-checkbox" aria-label="Toggle navigation menu"
```

### `<label>` — nav-hamburger

```
Location: inside <nav class="nav">, between .nav-brand and .nav-links
Attributes: for="nav-menu-toggle" class="nav-hamburger"
Children: 3× <span class="hamburger-line" aria-hidden="true">
```

---

## 7. Contracts

**N/A** — This feature has no external interfaces, API contracts, or inter-system communication. It is a self-contained visual enhancement to a static HTML page.

---

## 8. Palette Compliance Audit

| New Value                                                        | Derives From                         | Compliant |
| ---------------------------------------------------------------- | ------------------------------------ | --------- |
| `rgb(var(--color-slate-rgb) / var(--lg-nav-overlay-bg-opacity))` | `--color-slate`                      | ✅        |
| `var(--lg-glass-surface)`                                        | `--color-frost-rgb` (existing token) | ✅        |
| `var(--lg-glass-border)`                                         | `--color-frost-rgb` (existing token) | ✅        |
| `var(--lg-glass-shadow-md)`                                      | `--color-slate-rgb` (existing token) | ✅        |
| `var(--lg-color-text-primary)`                                   | `--color-frost` (existing token)     | ✅        |

No new hex, rgb, hsl, or named color values introduced. Principle III ✅.
