# Context Snapshot — Metamatopoeia Layout Refinement

**Generated**: 2026-06-06  
**Phase**: End of Phase 4 (Clarify) — Pre-Phase 5 Handoff  
**Purpose**: Restore full session context for a new model with zero chat history.

---

## What We Are Building

A targeted refinement of the existing `index.html` + `index.css` to more precisely match the layout, components, and color palette of the reference site `sds-smith/html_portfolio` (`public/` directory). The site is already built; this is a **surgical polish pass** covering exactly 5 changes. All constitution constraints remain in force.

**Deliverables**: Modified `index.html` + `index.css` only. No new files, no asset changes.

---

## Reference Repository — Key Facts

- Files live in `public/`: `index.html`, `index.css`, `portfolio.html`, `about.html`
- Background: `body::before` pseudo-element, `background-size: auto 100vh`, `background-position: right`, `background-repeat: no-repeat` → image is right-justified and does NOT fill full width
- Header: no `background` or `backdrop-filter` — transparent, same visual surface as page content behind it
- Portfolio cards (`portfolio.html`): `article.card.card-elevated` → `img.media` + `div.content` → (`span.project-title`, `p.portfolio-description`, `div.card-actions` → `a` > `button.button`)
- `card-elevated` in reference: `border-radius: 24px; overflow: hidden;` — **no padding on the card itself**; padding lives on `.content` (`padding: 16px`)
- `card-elevated::after` pseudo-element carries `background: var(--lg-glass-reflection)` (inset, absolute)
- SpeedDial FAB: `<button class="fab">` with `<span class="fab-icon visible">` + `<span class="fab-icon hidden">` inside; main icon is a paper plane (send/arrow SVG)
- SpeedDial actions: `div.actions` > `div.action-wrapper` > `a.action`

---

## 5 Changes — Discovery Notes

### Req 1 — Background Image Treatment

**Model**: `body::before { content: ""; position: fixed; top: 0; left: 0; right: 0; bottom: 0; z-index: -1; background: linear-gradient(overlay), url("...bg.jpeg"), fallback-gradient; background-size: auto 100vh; background-position: right; background-repeat: no-repeat; will-change: transform; pointer-events: none; }`

**Current**: `body { background-image: linear-gradient(...), url("..."); background-size: cover; background-position: center; background-attachment: fixed; }` — fills full width, centered.

**Change**: Move background from `body` to `body::before` pseudo-element with model's `background-size: auto 100vh; background-position: right; background-repeat: no-repeat`. Overlay color must remain derived from `--color-slate-rgb` per constitution.

**Constitution check**: Using `rgba(0,0,0,...)` for overlay would violate palette constraint — must stay `rgb(var(--color-slate-rgb) / 0.85)`.

---

### Req 2 — Header Surface Unification

**Model**: `header { pointer-events: none; }` — no background, no backdrop-filter, no border. Header is visually transparent against the page background.

**Current**: `.header { background: var(--lg-glass-surface); backdrop-filter: blur(...); border-bottom: 1px solid var(--lg-glass-border); }` — distinct glass surface.

**Change**: Remove `background`, `backdrop-filter`, `-webkit-backdrop-filter`, and `border-bottom` from `.header`. Keep `pointer-events: none` and add `header * { pointer-events: auto; }` per model pattern.

---

### Req 3 — Workshop Card Exact Design Match

**Model card structure** (from `portfolio.html`):

```html
<article class="card card-elevated">
  <img src="..." class="media" alt="..." />
  <div class="content">
    <span class="project-title">Title</span>
    <p class="portfolio-description">...</p>
    <div class="card-actions">
      <a href="..." target="_blank" rel="noopener noreferrer">
        <button class="button">View Source Code</button>
      </a>
    </div>
  </div>
</article>
```

**Current card structure**: Uses `.project-image`, `.project-content`, `.project-actions`, `.project-title`, `.project-description` — all custom classes.

**CSS changes needed**:

- `card-elevated`: remove `padding: 2rem`; set `border-radius: 24px`; set `overflow: hidden`; move `::before` reflection to `::after` inset pseudo-element
- Add `.media` class (matches reference)
- Add `.content` class: `position: relative; z-index: 1; padding: 16px;`
- Add `.card-actions` class (distinct from current `.project-actions`)

**HTML changes needed**: Remap class names in all 4 workshop cards. Use `<span class="project-title">` (not `<h3>`). Wrap each action button in `<a>...<button class="button">...</button></a>`.

**Deviation noted**: Model's `<a><button>` pattern is technically invalid HTML (interactive element inside interactive element). Since the user requires exact match, this is acceptable but must be documented.

---

### Req 4 — Color Palette Token Reassignment

**Two sub-changes**:

1. `--lg-color-text-primary` (or equivalent token driving body/element text color) must use `var(--color-frost)` — light text on dark background. Currently uses `var(--color-slate)` (dark text).

2. `--lg-glass-reflection` must use `--color-slate` based gradient. Currently: `linear-gradient(135deg, rgb(var(--color-frost-rgb) / 0.4) 0%, rgb(var(--color-frost-rgb) / 0) 50%)`. Change to: `linear-gradient(135deg, rgb(var(--color-slate-rgb) / 0.4) 0%, rgb(var(--color-slate-rgb) / 0) 50%)` (or matching model's four-stop pattern using slate-rgb).

**Cascade effects**: Changing `text-primary` to `--color-frost` will make nav links, section titles, hero title, project titles all light-colored. Dark mode override in `@media (prefers-color-scheme: dark)` may need review (currently already sets `text-primary` to `--color-frost`).

---

### Req 5 — Speed Dial FAB Icon

**Model icon** (paper plane / send):

```svg
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <line x1="22" y1="2" x2="11" y2="13"></line>
  <polygon points="22 2 15 22 11 13 2 9 22 2"></polygon>
</svg>
```

**Current icon**: Two intersecting lines ("+") that rotate to "×" when open.

**Change**:

- Replace SVG path with paper plane icon above
- Remove CSS rule `.speed-dial-checkbox:checked ~ .speed-dial-fab .speed-dial-icon { transform: rotate(45deg); }` — icon must NOT change on open state

---

## Phase 4 Gap Resolutions (Not in Discovery Notes)

These were identified during the Phase 4 cold-eye review and added to the spec as FR-012a, FR-020a, FR-020b.

### G1: Hero + Contact Card Padding (FR-012a)

- `card-elevated` loses `padding: 2rem` globally so images bleed to card edges.
- `.hero-card` and `.contact-card` must each explicitly add `padding: 2rem` back in CSS.
- Without this, the hero tagline and contact list items would be flush against the card border.

### G2: Base `.button` Glass Style (FR-020a)

- Workshop card buttons change from `<a class="button button-secondary">` to `<a class="button">`.
- The current base `.button` has no background/border without a modifier — renders invisible.
- Resolution: update base `.button` to glass-surface style: `border: 1px solid var(--lg-glass-border); background: var(--lg-glass-surface); color: var(--lg-color-text-muted);`
- `.button-primary` override is unaffected (Hero CTA stays teal).
- `.button-secondary` CSS rule stays in the file but is no longer applied to any element.

### G3: Full-Width Cards in List (FR-020b)

- Added `.layout-list .card { width: 100%; }` to guarantee full-width cards in the flex-column layout.

---

## Discovery Q&A Summary (Phase 2 Intent Locks)

| Question                              | Decision                                                       |
| ------------------------------------- | -------------------------------------------------------------- |
| `<a><button>` vs `<a class="button">` | `<a class="button">` — semantic compliance, visually identical |
| `<h3>` vs `<span>` for project title  | `<h3 class="project-title">` — preserve heading hierarchy      |
| Dark mode `text-primary` override     | Keep the override explicitly even though now redundant         |

---

## Phase 5 Handoff Brief (for SWE-1.6 bootstrapping)

The next model starts a **new Cascade session**, selects **SWE-1.6**, and runs `/speckit.plan`.

### Codebase State

- `index.html` — fully built, 140 lines, single page, 3 sections + speed dial FAB
- `index.css` — fully built, 646 lines, complete Liquid Glass design system
- `assets/` — all 4 card media images present; `background-image-profile.jpeg` present

### Files to Edit

1. **`index.css`** — ~20 targeted rule changes (background, header, card classes, color tokens, button, FAB CSS)
2. **`index.html`** — workshop card HTML restructure (4 cards × class/element remapping) + FAB SVG swap

### Files NOT to Touch

- `assets/` directory
- `<script>` block in `index.html` (FAB JS logic unchanged)
- Section structure, nav, hero content, contact content

### Key CSS Change Map

| Target                                                                                                             | Change                                                                           |
| ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------- |
| `body`                                                                                                             | Remove background declarations; keep `min-height: 100vh` + color                 |
| `body::before` (new)                                                                                               | Add pseudo-element with right-justified bg image                                 |
| `@media (max-width: 768px)` `body::before`                                                                         | Add `background-size: cover`                                                     |
| `.header`                                                                                                          | Remove background, backdrop-filter, border-bottom; add `pointer-events: none`    |
| `.header *` (new)                                                                                                  | Add `pointer-events: auto`                                                       |
| `--lg-color-text-primary`                                                                                          | Change to `var(--color-frost)`                                                   |
| `--lg-glass-reflection`                                                                                            | Change to 4-stop slate-rgb gradient                                              |
| `.card::before`                                                                                                    | Replace with `.card::after` (inset, full-card)                                   |
| `.card-elevated::before`                                                                                           | Replace with `.card-elevated::after` (inset)                                     |
| `.card-elevated`                                                                                                   | Add `overflow: hidden`; change `border-radius` to `24px`; remove `padding: 2rem` |
| `.hero-card`                                                                                                       | Add `padding: 2rem`                                                              |
| `.contact-card`                                                                                                    | Add `padding: 2rem`                                                              |
| `.media` (new)                                                                                                     | Add full-width image rule                                                        |
| `.content` (new)                                                                                                   | Add `z-index: 1; padding: 16px`                                                  |
| `.card-actions` (new)                                                                                              | Add flex row rule                                                                |
| `.portfolio-links` (new)                                                                                           | Add `display: flex; flex-direction: column; gap: 24px;`                          |
| `.layout-list .card` (new)                                                                                         | Add `width: 100%`                                                                |
| `.button`                                                                                                          | Update to glass-surface style                                                    |
| `.speed-dial-checkbox:checked ~ .speed-dial-fab .speed-dial-icon`                                                  | Remove rotation rule                                                             |
| `.project-grid`, `.project-card`, `.project-image`, `.project-content`, `.project-actions`, `.project-description` | Remove all six rules                                                             |

---

## Constraints Checklist (carry-forward)

- [x] Zero external network requests
- [x] No build tools, frameworks, or preprocessors
- [x] Single `index.html` + single `index.css`
- [x] Exactly 3 sections: Hero, Workshop, Contact
- [x] Nav: home / workshop / contact
- [x] Brand: Metamatopoeia only
- [x] 4-color palette enforced (all changes must trace to 4 tokens)
- [x] `--lg-{group}-{subgroup}-{token}` naming
- [x] JS ≤ 30 non-empty source lines, single `<script>` block
- [x] Semantic HTML5
- [x] `clamp()` for fluid typography
- [x] Mobile-first, 768px breakpoint
- [x] `prefers-color-scheme`, `prefers-reduced-motion`, `prefers-reduced-transparency` handled
- [x] Decorative background image permitted by constitution v1.0.2
