# Context Snapshot — Metamatopoeia Website

**Generated**: 2026-06-05  
**Phase**: End of Phase 4 (Clarify) — Pre-Phase 5 Handoff  
**Purpose**: Restore full session context for a new model with zero chat history.

---

## What We Are Building

A single-page brand website for **Metamatopoeia** — a software project showcase. The site is a **constitutional remix** of the reference repository `sds-smith/html_portfolio` (at `github.com/sds-smith/html_portfolio`, files in `public/`). We keep the entire CSS architecture and component system verbatim, then remap brand, palette, structure, and content to satisfy the Metamatopoeia constitution.

**Deliverables**: `index.html` + `index.css` (+ `assets/` directory of images).

---

## Reference Repository — Key Facts

- Files live in `public/`: `index.html`, `index.css`, `about.html`, `portfolio.html`
- `index.html` = featured project hero layout (`.layout-fullscreen` + `.layout-hero`)
- `portfolio.html` = project list (`.layout-list` scroll container)
- `about.html` = personal philosophy cards
- Shared `index.css` (~19KB) covers all pages
- Background: `./assets/background-image-profile.jpeg` + `rgba(0,0,0,0.7)` overlay via CSS `body::before`
- JS: one `<script>` block at bottom of `<body>` — exactly the SpeedDial FAB logic, ~28 lines

---

## Architecture Decisions (Not in Spec)

### Single-Page Collapse Strategy
The reference has 3 pages. We collapse them into 1:
- `index.html` hero card → `#hero` section
- `portfolio.html` project list → `#workshop` section  
- `about.html` → **dropped entirely** (no bio content permitted)
- New → `#contact` section (dedicated, using `.card-elevated` glass card)

The `.scrollable` fixed-position scroll container from the reference applies within `<main>` to preserve the scroll-behind-sticky-header experience.

### Layout Class Application
- `<body>` keeps `class="layout-fullscreen"` for the overall viewport lock
- `#hero` uses `.layout-hero` centering
- `#workshop` and `#contact` use `.layout-list` with the `.scrollable` container

### Color Palette Remapping
The reference uses a dark grayscale palette. We replace ALL palette token values with Metamatopoeia colors:

| Reference token | Reference value | → | Metamatopoeia mapping |
|---|---|---|---|
| `--lg-palette-primary-dark` | `#09090b` | → | `--color-slate: #5A606A` |
| `--lg-palette-primary-main` | `#18181b` | → | `--color-slate: #5A606A` |
| `--lg-palette-text-on-surface` | `#fff` | → | `--color-frost: #EFF1F3` |
| `--lg-palette-text-on-surface-secondary` | `#d4d4d8` | → | `--color-mist: #BDBFC6` |
| `--lg-palette-background-fallback` gradient | dark zinc | → | gradient using `--color-slate` + `--color-teal` |

The four palette tokens are declared in `:root`:
```css
--color-slate: #5A606A;
--color-teal:  #79A1A2;
--color-mist:  #BDBFC6;
--color-frost: #EFF1F3;
```
All `--lg-palette-*` tokens then reference these via CSS custom property composition.

**Glass physics tokens stay unchanged** (`--lg-glass-blur`, `--lg-glass-bg-opacity`, etc.) — only the color palette underneath them changes.

### SpeedDial FAB — Contact Channels
Reference had: Email, StackOverflow, GitHub, LinkedIn (4 actions).  
Ours has: **Email, GitHub, LinkedIn only** (3 actions — StackOverflow dropped).

Updated links:
- Email → `mailto:metamatopoeia@gmail.com`
- GitHub → `https://github.com/metamatopoeia`
- LinkedIn → `https://www.linkedin.com/company/metamatopoeia`

### AboutApp Component — Dropped
The reference's bottom-left `?` popover (`.about-app`) contains "© 2026 Shawn D Smith". It is entirely removed — no replacement.

### Background Image — Documented Deviation
`background-image-profile.jpeg` is retained as decorative background (same as reference). This is a documented deviation from the constitution's "no personal-identity content" clause. Justification: purely decorative visual asset, no personal name in caption or markup. Must appear in `plan.md` Complexity Tracking table.

---

## Content Inventory

### Hero Section (`#hero`)
- Layout: `.layout-hero` centered `.card-elevated`
- Tagline: **"Elegant Software Intentionally Designed"**
- CTA button: "See the Workshop" → `href="#workshop"`
- No media image (text-only card)

### Workshop Section (`#workshop`)
Four `.card-elevated` project cards in `.layout-list`:

| Project | Media Image | Description Source | Action Buttons |
|---|---|---|---|
| Cup | `card-media-cup.png` | Reference `index.html` (desktop + mobile variants) | "Check out the Gist" → Gist URL; "Check out the Preview" → cupsocial.app; "Join the Beta" → cupsocial.app |
| Liquid Glass UI | `card-media-liquid-glass-ui.png` | Reference `portfolio.html` | "View Source Code" → github.com/metamatopoeia/liquid-glass-ui |
| Discover Breweries | `card-media-discover-breweries.png` | Reference `portfolio.html` | "View Source Code" → github.com/sds-smith/discover-breweries |
| Assemble the Jams | `card-media-atj.png` | Reference `portfolio.html` | "View Source Code" → github.com/sds-smith/assemble_the_jams_3 |

### Contact Section (`#contact`)
- Layout: `.layout-list` single `.card-elevated`
- Section heading: "Contact"
- Three channel rows, each with SVG icon + label + link:
  - Email → `mailto:metamatopoeia@gmail.com`
  - GitHub → `https://github.com/metamatopoeia` (new tab)
  - LinkedIn → `https://www.linkedin.com/company/metamatopoeia` (new tab)

---

## Assets Required

All sourced from `sds-smith/html_portfolio` `public/assets/`:
- `background-image-profile.jpeg`
- `card-media-cup.png`
- `card-media-liquid-glass-ui.png`
- `card-media-discover-breweries.png`
- `card-media-atj.png`
- `metamatopoeia_logo.jpeg` (favicon)

The `plan.md` implementation steps must include a task to copy these from the reference repo into `./assets/`.

---

## What Is NOT Changing from the Reference CSS

These are kept **verbatim** (only palette token values swap):
- All glass physics tokens (`--lg-glass-*`)
- Card classes: `.card`, `.card-elevated`, `.card-outlined`
- Layout classes: `.layout-fullscreen`, `.layout-hero`, `.layout-list`
- SpeedDial CSS: `.speed-dial`, `.fab`, `.action`, `.action-wrapper`, `.fab-icon`
- Responsive breakpoints: `@media (max-width: 768px)`, `@media (max-height: 500px) and (orientation: landscape)`
- `prefers-reduced-motion`, `prefers-reduced-transparency`, `prefers-color-scheme: dark` blocks
- `.scrollable` fixed-position scroll container
- `.desktop` / `.mobile` visibility toggle classes
- Button, typography, nav, header styles

---

## Constraints Checklist (for plan.md Constitution Check gate)

- [x] Zero external network requests
- [x] No build tools, frameworks, or preprocessors
- [x] Single `index.html` + single `index.css`
- [x] Exactly 3 sections: Hero, Workshop, Contact
- [x] Nav: home / workshop / contact
- [x] Brand: Metamatopoeia only
- [x] 4-color palette enforced
- [x] `--lg-{group}-{subgroup}-{token}` naming
- [x] JS ≤ 30 lines, single `<script>` block
- [x] Semantic HTML5
- [x] `clamp()` for fluid typography
- [x] Mobile-first, 768px breakpoint
- [x] `prefers-color-scheme`, `prefers-reduced-motion`, `prefers-reduced-transparency` handled
- [ ] Background image deviation → documented in plan.md Complexity Tracking
