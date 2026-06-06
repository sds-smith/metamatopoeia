# Feature Specification: Metamatopoeia Layout Refinement

**Feature Branch**: `002-layout-refinement`  
**Created**: 2026-06-06  
**Status**: Clarified  
**Input**: Refine page layout, components, and color palette to more precisely mirror the model site `sds-smith/html_portfolio`. Five targeted changes: (1) right-justified background image treatment, (2) header surface unification with page content, (3) workshop card exact design match to portfolio cards, (4) color palette token reassignment (`color-frost` → `text-primary`, `color-slate` → `lg-glass-reflection`), (5) speed dial FAB paper plane icon, static across open/closed states.

---

## User Scenarios & Testing _(mandatory)_

### User Story 1 — Background Image Renders Right-Justified (Priority: P1)

A visitor loads the page and sees the background image displayed in the right portion of the viewport, with the left side showing only the gradient overlay — exactly matching the model site's composition.

**Why this priority**: The background treatment is the dominant visual impression of the page. It establishes the depth and atmosphere for all glass cards layered on top.

**Independent Test**: Open `index.html` from the filesystem in a wide viewport (≥1024px). The background image appears on the right side; the left side fades into the gradient overlay. On mobile, the background falls back to `cover` per the model's `@media (max-width: 768px)` override.

**Acceptance Scenarios**:

1. **Given** a visitor opens `index.html` at a desktop viewport, **When** the page loads, **Then** the background image is positioned to the right and does not fill the full viewport width.
2. **Given** the page is loaded, **When** the visitor resizes to ≤768px, **Then** the background image switches to `cover` mode filling the full viewport.
3. **Given** the page is loaded, **When** the visitor inspects the network tab, **Then** zero external network requests are made (local asset only).

---

### User Story 2 — Header Appears as Unified Page Surface (Priority: P1)

A visitor scrolls through the page and perceives the sticky header as part of the same transparent backdrop as the rest of the content — not as a distinct frosted panel floating above it.

**Why this priority**: The glass panel separation between header and content creates a layering artifact inconsistent with the model's aesthetic. Removing it produces the intended unified composition.

**Independent Test**: Open `index.html`. Scroll down past the Hero section. The header text and nav links remain readable but the header has no visible background fill, blur effect, or bottom border separating it from the content below.

**Acceptance Scenarios**:

1. **Given** the visitor views the page at any scroll position, **When** they look at the header, **Then** no glass surface, blur, or bottom border is visible in the header area.
2. **Given** a keyboard user tabs to a nav link, **When** focus lands on the link, **Then** the focus indicator is still visible (pointer-events retained on interactive children).

---

### User Story 3 — Workshop Cards Match Portfolio Card Design (Priority: P1)

A visitor browses the Workshop section and sees project cards that are visually identical in structure and styling to the portfolio cards on the model site.

**Why this priority**: The Workshop section is the primary showcase content. Its card design communicates the brand's visual quality.

**Independent Test**: Open `index.html#workshop`. Compare each card against `sds-smith/html_portfolio` `portfolio.html`. Card image bleeds to the top edges of the card. Content padding is inside a `.content` div. Buttons are inline anchor elements styled as buttons.

**Acceptance Scenarios**:

1. **Given** the visitor views a workshop card, **When** the card renders, **Then** the project image fills the full card width, bleeds to the top and side edges (no border-radius gap or padding between image and card edges), and is not padded inside the card.
2. **Given** the visitor views a workshop card, **When** they read the content area, **Then** the title, description, and action buttons are contained in a `.content` div with `16px` padding, separated from the image.
3. **Given** the visitor clicks an action button, **When** the link resolves, **Then** it navigates correctly (new tab for external URLs) and is keyboard-focusable.
4. **Given** the visitor is on mobile (≤768px), **When** the Workshop section renders, **Then** cards stack vertically and action buttons are full-width.

---

### User Story 4 — Color Palette Uses Frost for Text and Slate for Reflection (Priority: P2)

A visitor reads the page and sees light-colored text rendered against the dark background overlay, with glass card reflections using a slate-toned gradient.

**Why this priority**: The text-primary token was incorrectly set to `--color-slate` (dark), rendering dark text on a dark background. The reflection token should match the brand's neutral tone.

**Independent Test**: Open `index.html`. All body text, nav links, section titles, card titles, and descriptions appear in the light `--color-frost` (`#EFF1F3`) tone. Inspect the glass cards — the reflection gradient at the top-left corner uses a slate-toned (not white or frost-toned) gradient.

**Acceptance Scenarios**:

1. **Given** the visitor views the page in light mode, **When** they read any text element using `--lg-color-text-primary`, **Then** the text color resolves to `var(--color-frost)`.
2. **Given** a keyboard user with `prefers-color-scheme: dark` active, **When** the dark mode override applies, **Then** `--lg-color-text-primary` is still explicitly set to `var(--color-frost)` in the dark mode block.
3. **Given** a glass card is rendered, **When** the visitor inspects the `::after` reflection pseudo-element, **Then** its gradient resolves to `rgb(var(--color-slate-rgb) / ...)` stop values.

---

### User Story 5 — Speed Dial FAB Shows Paper Plane Icon (Static) (Priority: P2)

A visitor clicks the Speed Dial FAB and sees a paper plane icon that remains unchanged whether the dial is open or closed.

**Why this priority**: The current "+" icon that rotates to "×" is inconsistent with the model site's send/contact metaphor and changes state in a distracting way.

**Independent Test**: Open `index.html`. The FAB displays a paper plane (Lucide `Send`) icon. Click it to open — the icon does not change or rotate. Close it — the icon is still the paper plane.

**Acceptance Scenarios**:

1. **Given** the FAB is closed, **When** the visitor views it, **Then** the FAB icon is a paper plane SVG (`<line x1="22" y1="2" x2="11" y2="13">` + `<polygon points="22 2 15 22 11 13 2 9 22 2">`).
2. **Given** the FAB is open, **When** the visitor views it, **Then** the icon is identical to the closed state — no rotation, no swap.
3. **Given** a reduced-motion user, **When** the FAB is toggled, **Then** no icon transform occurs (transition duration is 0ms).

---

### Edge Cases

- At narrow viewports (<320px), the background `auto 100vh` sizing must not cause horizontal overflow — the `body` base fallback color (`--lg-color-surface-page`) fills the gap.
- If both `prefers-reduced-motion` and `prefers-reduced-transparency` are active simultaneously, both overrides apply without conflict.
- Removing `backdrop-filter` from `.header` must not cause any stacking context side-effects on the nav links' `z-index` relative to page content.
- The `body::before` `will-change: transform` declaration must not be present if it causes compositing issues on low-end devices — it is acceptable to omit.

---

## Requirements _(mandatory)_

### Functional Requirements

#### REQ-1: Background Image Treatment

- **FR-001**: The background MUST be rendered via a `body::before` pseudo-element — `content: ""; position: fixed; top: 0; left: 0; right: 0; bottom: 0; z-index: -1; pointer-events: none;`
- **FR-002**: The `body::before` `background` shorthand MUST use exactly three layers: `linear-gradient(overlay, overlay)`, `url("./assets/background-image-profile.jpeg")`, and a CSS fallback gradient — in that order.
- **FR-003**: The overlay color in the gradient MUST derive from `rgb(var(--color-slate-rgb) / 0.85)` — no raw `rgba(0,0,0,...)` values permitted.
- **FR-004**: `background-size: auto 100vh`, `background-position: right`, `background-repeat: no-repeat` MUST be set on `body::before`.
- **FR-005**: The `body` rule MUST have its `background-image`, `background-size`, `background-position`, and `background-attachment` declarations removed; `body` retains only `min-height: 100vh` and color fallback.
- **FR-006**: At `@media (max-width: 768px)`, `body::before { background-size: cover; }` MUST be set to restore full-width coverage on mobile.

#### REQ-2: Header Surface Unification

- **FR-007**: `.header` MUST NOT have `background`, `backdrop-filter`, or `-webkit-backdrop-filter` declarations.
- **FR-008**: `.header` MUST NOT have a `border-bottom` declaration.
- **FR-009**: `.header` MUST have `pointer-events: none`.
- **FR-010**: A `header *` (or `.header *`) rule MUST set `pointer-events: auto` so nav links and brand text remain interactive.

#### REQ-3: Workshop Card Design Match

- **FR-011**: Each workshop card MUST use the HTML structure: `<article class="card card-elevated">` → `<img class="media" ...>` + `<div class="content">` → (`<h3 class="project-title">`, `<p class="portfolio-description">`, `<div class="card-actions">` → `<a class="button" href="...">` elements).
- **FR-012**: `.card-elevated` MUST have `border-radius: 24px` and `overflow: hidden`. The `padding: 2rem` declaration MUST be removed from `.card-elevated`.
- **FR-012a**: Because `.card-elevated` loses its global padding, `.hero-card` MUST add `padding: 2rem` explicitly in `index.css` to preserve the Hero section card layout. `.contact-card` MUST likewise add `padding: 2rem` explicitly.
- **FR-013**: `.card-elevated::before` MUST be replaced by `.card-elevated::after` with `content: ""; position: absolute; inset: 0; border-radius: inherit; background: var(--lg-glass-reflection); pointer-events: none;`
- **FR-014**: The `.card::before` declaration MUST likewise be replaced by `.card::after` using the same inset pattern.
- **FR-015**: A `.media` CSS rule MUST be added: `position: relative; z-index: 1; display: block; width: 100%; object-fit: cover; background-size: cover; background-repeat: no-repeat; background-position: center;`
- **FR-016**: A `.content` CSS rule MUST be added: `position: relative; z-index: 1; padding: 16px;`
- **FR-017**: A `.card-actions` CSS rule MUST be added: `display: flex; gap: 8px; margin-top: 16px; flex-wrap: wrap;`
- **FR-018**: The `.project-image`, `.project-content`, `.project-actions`, and `.project-description` CSS rules MUST be removed. The `.project-card` CSS rule MUST be removed.
- **FR-019**: The `.project-grid` layout wrapper MUST be replaced by `.portfolio-links` (flex column, `gap: 24px`) to match the model's list layout for portfolio cards.
- **FR-020**: Action buttons in workshop cards MUST use `<a class="button" href="...">` — no `<button>` elements nested inside anchors, and no `.button-secondary` modifier class on workshop card links.
- **FR-020a**: The base `.button` CSS rule MUST be updated to match the model's glass-surface button style: `border: 1px solid var(--lg-glass-border); background: var(--lg-glass-surface); color: var(--lg-color-text-muted); font-size: var(--lg-typography-size-xs); padding: 8px 16px;`. The `.button-primary` override retains its teal-background style. The `.button-secondary` CSS rule MAY be retained in the stylesheet for future use but is not applied to any HTML element in v1.
- **FR-020b**: A `.layout-list .card` rule MUST be added: `width: 100%;` — ensures cards are full-width in the flex-column portfolio list across all browsers.

#### REQ-4: Color Palette Token Reassignment

- **FR-021**: `--lg-color-text-primary` in `:root` MUST be set to `var(--color-frost)`.
- **FR-022**: `--lg-glass-reflection` in `:root` MUST use `--color-slate-rgb` channel values. The gradient MUST follow the model's four-stop pattern: `linear-gradient(135deg, rgb(var(--color-slate-rgb) / 0.4) 0%, rgb(var(--color-slate-rgb) / 0) 40%, rgb(var(--color-slate-rgb) / 0) 60%, rgb(var(--color-slate-rgb) / 0.15) 100%)`.
- **FR-023**: The `@media (prefers-color-scheme: dark)` block MUST retain `--lg-color-text-primary: var(--color-frost)` explicitly (no removal of the override even though it is now redundant with the `:root` value).

#### REQ-5: Speed Dial FAB Icon

- **FR-024**: The FAB icon SVG in `index.html` MUST be replaced with the paper plane (send) icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="22" y1="2" x2="11" y2="13"></line><polygon points="22 2 15 22 11 13 2 9 22 2"></polygon></svg>`.
- **FR-025**: The CSS rule `.speed-dial-checkbox:checked ~ .speed-dial-fab .speed-dial-icon { transform: rotate(45deg); }` MUST be removed from `index.css`.

#### Carry-Forward Constitutional Requirements (unchanged from 001)

- **FR-026**: All color values MUST derive from the four palette tokens: `--color-slate`, `--color-teal`, `--color-mist`, `--color-frost`.
- **FR-027**: All Liquid Glass design tokens MUST follow `--lg-{group}-{subgroup}-{token}` naming.
- **FR-028**: The JavaScript `<script>` block MUST remain ≤30 non-empty source lines.
- **FR-029**: Zero external network requests permitted.
- **FR-030**: Semantic HTML5 elements, `clamp()` typography, mobile-first 768px breakpoint, and accessibility ARIA labels must all be preserved.

---

### Key Entities

- **`body::before`**: Fixed pseudo-element carrying the background image and overlay gradient. Sole owner of background rendering.
- **`.media`**: Block-level image class that bleeds to card edges with no internal padding, matching the reference's card image treatment.
- **`.content`**: Padding container within `.card-elevated` that houses all card text and actions at `16px` inset.
- **`.card-actions`**: Flex row container for action `<a class="button">` links inside a card.
- **`.portfolio-links`**: Flex column container for workshop cards, replacing `.project-grid`.
- **`--lg-color-text-primary`**: Now resolves to `var(--color-frost)` — light text against the dark-overlaid background.
- **`--lg-glass-reflection`**: Now uses `--color-slate-rgb` four-stop gradient for the glass sheen effect.

---

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: At a viewport of ≥1024px, the background image is visibly confined to the right portion of the viewport; the left side shows only the gradient overlay. Verified by visual inspection.
- **SC-002**: Scrolling past the Hero section, the header area shows no glass background, blur, or bottom border. Verified by visual inspection and DevTools computed styles.
- **SC-003**: All four workshop card images bleed flush to the top and side edges of their cards (no padding gap between image and card border). Verified by visual inspection.
- **SC-004**: Each workshop card's content area has exactly `16px` padding (DevTools box model). Action buttons are `<a class="button">` elements, not `<button>` or `<a><button>`.
- **SC-005**: All text using `--lg-color-text-primary` renders in `#EFF1F3` (frost) in light mode. Verified by DevTools computed color value.
- **SC-006**: `--lg-glass-reflection` computed value contains `rgb(90 96 106 / ...)` stops (slate-rgb), not frost-rgb. Verified by DevTools.
- **SC-007**: The FAB icon is a paper plane SVG. Clicking the FAB to open does not change or rotate the icon. Verified by visual inspection in open and closed states.
- **SC-008**: `index.html` opens and renders without console errors from the filesystem.
- **SC-009**: Zero external network requests on cold load (DevTools Network tab).
- **SC-010**: The JavaScript `<script>` block contains ≤30 non-empty source lines (unchanged from 001).
- **SC-011**: The page renders without horizontal overflow at 280px–1440px viewport widths.

---

## Assumptions

- All five changes are surgical edits to the existing `index.html` and `index.css`. No new files or assets are added.
- The existing `assets/` directory and all four card media images (`card-media-cup.png`, etc.) are already present.
- The `.project-grid` wrapper in `index.html` is replaced with `.portfolio-links`; the section structure (`<section id="workshop" ...>`, `<h2 class="section-title">`) is unchanged.
- The `.speed-dial-fab` label element in `index.html` retains its current `for`, `aria-label`, and `tabindex` attributes; only the inner SVG is replaced.
- `--color-slate-rgb: 90 96 106` is already declared in `:root` and can be referenced by the new `--lg-glass-reflection` value.
- The dark mode block currently sets `--lg-color-text-primary: var(--color-frost)` — this override is retained verbatim per Q3 intent lock.
- The `body::before` fallback gradient uses `--color-slate` and `--color-teal` token-derived values (no raw hex permitted).
