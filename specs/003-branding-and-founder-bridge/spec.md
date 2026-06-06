# Feature Specification: Branding Enhancement & Founder Bridge

**Feature Branch**: `003-branding-and-founder-bridge`
**Created**: 2026-06-06
**Status**: Clarified
**Input**: Enhance branding by integrating the Metamatopoeia logo mark (nav brand + background watermark), replace the hero button CTA with an asymmetric dual-text typographic link pair ("Scroll to Workshop" | "Meet the Founder"), and add a persistent nav link bridging to the personal website at https://sds-smith.io.

---

> ⚠️ **Constitutional Pre-Condition — Principle IV Amendment Required (v2.0.0 → v2.1.0)**
> The current constitution prohibits navigation from referencing anything other than the three in-page anchors. The "meet the founder" nav link requires a MINOR amendment to Principle IV to permit external attribution links alongside the three required in-page anchors. **This amendment MUST be ratified and committed before implementation begins.**

---

## User Scenarios & Testing _(mandatory)_

### User Story 1 — Logo Mark Appears in Nav Brand (Priority: P1)

A visitor opens the page and sees the Metamatopoeia diamond mark logo in the header navigation area, replacing the plain text "Metamatopoeia" brand identifier.

**Why this priority**: The nav brand is the first persistent identity element visible at every scroll position. Replacing text with the recognizable logo mark immediately establishes visual brand identity and is a prerequisite visual anchor for the whole feature.

**Independent Test**: Open `index.html` from the filesystem. The header navigation area displays the logo image (`Metamatopoeia_simple_small.png`) rather than the text string "Metamatopoeia".

**Acceptance Scenarios**:

1. **Given** a visitor opens the page, **When** the header renders, **Then** the nav-brand area shows the logo image, not the plain text "Metamatopoeia".
2. **Given** the logo image fails to load, **When** the browser falls back, **Then** `alt="Metamatopoeia"` text is displayed in place of the image.
3. **Given** a screen reader user navigates the header, **When** focus reaches the nav brand, **Then** the logo image is announced as "Metamatopoeia".
4. **Given** a mobile viewport (≤768px), **When** the header renders, **Then** the logo is visible and proportionally sized without overflowing the nav row.

---

### User Story 2 — Hero Asymmetric Dual-Text CTA (Priority: P1)

A visitor lands on the hero section and sees two elegant, typographic text-only links below the tagline — "Scroll to Workshop" and "Meet the Founder" — separated by a minimal divider. There is no button shape.

**Why this priority**: This is the primary above-the-fold conversion surface. The typographic treatment communicates brand sophistication; "Meet the Founder" is the hero's personal bridge entry point.

**Independent Test**: Open `index.html`. The hero card contains no button-shaped element. Below the tagline, two inline text links are visible separated by a divider. Clicking "Scroll to Workshop" scrolls to `#workshop`; clicking "Meet the Founder" opens `https://sds-smith.io` in a new tab.

**Acceptance Scenarios**:

1. **Given** a visitor views the hero section, **When** the page loads, **Then** no element with class `.button` or `.button-primary` is present inside the hero card.
2. **Given** the visitor sees the hero card, **When** they read the CTA area, **Then** two inline text links — "Scroll to Workshop" and "Meet the Founder" — are visible, separated by a decorative divider.
3. **Given** the visitor clicks "Scroll to Workshop", **When** the link resolves, **Then** the page navigates to the `#workshop` section in the same tab.
4. **Given** the visitor clicks "Meet the Founder", **When** the link opens, **Then** `https://sds-smith.io` opens in a new browser tab.
5. **Given** a keyboard user tabs through the hero card, **When** focus reaches the CTA area, **Then** both links are individually focusable and keyboard-activated; the divider span is skipped by focus order.

---

### User Story 3 — "Meet the Founder" Persistent Nav Link (Priority: P1)

A visitor browsing any section of the page can navigate to the personal website directly from the persistent navigation bar via a "meet the founder" link, without returning to the hero section.

**Why this priority**: Persistent nav placement ensures the founder bridge is accessible at every scroll position, not only from the hero viewport.

**Independent Test**: Open `index.html`. The nav bar shows four links: home, workshop, meet the founder, contact — in that order. Clicking "meet the founder" opens `https://sds-smith.io` in a new tab.

**Acceptance Scenarios**:

1. **Given** the visitor views the header nav, **When** the page loads, **Then** four nav links are visible in order: home, workshop, meet the founder, contact.
2. **Given** the visitor clicks "meet the founder" in the nav, **When** the link activates, **Then** `https://sds-smith.io` opens in a new tab with `rel="noopener noreferrer"`.
3. **Given** a keyboard user tabs through the nav, **When** focus reaches "meet the founder", **Then** the link is keyboard-focusable and activatable.
4. **Given** a mobile viewport (≤768px), **When** the nav renders, **Then** the four links display without horizontal overflow or obscured text.

---

### User Story 4 — Background Logo Watermark (Priority: P2)

A visitor views the hero section at a desktop viewport and notices the Metamatopoeia logo mark faintly present in the left background area — the space not occupied by the profile photo — as a large, atmospheric watermark.

**Why this priority**: Purely decorative brand atmosphere. The site is fully functional without it; it is a visual polish layer that deepens brand presence in negative space.

**Independent Test**: Open `index.html` at ≥1024px viewport. In the left portion of the hero background (the gradient-overlaid area), the transparent logo mark is visible as a large watermark. Network tab shows zero external requests.

**Acceptance Scenarios**:

1. **Given** a desktop viewport (≥1024px), **When** the background renders, **Then** the logo watermark is visible in the left background area without overlapping or distracting from the foreground glass card content.
2. **Given** the page loads, **When** the visitor checks the Network tab, **Then** zero external network requests are made (local asset only).
3. **Given** a screen reader user navigates the page, **When** the page structure is announced, **Then** the background watermark is not exposed to the accessibility tree (CSS background image, not `<img>`).
4. **Given** a mobile viewport (≤768px), **When** the background renders, **Then** the watermark is suppressed or scales gracefully to avoid competing with primary content.

---

### Edge Cases

- At narrow viewports (<320px), four nav links must not cause horizontal overflow — verify wrapping or reduced font-size behavior.
- If `Metamatopoeia_simple_small.png` fails to load, the `alt="Metamatopoeia"` text fallback must preserve nav brand readability.
- The hero CTA divider `<span>` MUST carry `aria-hidden="true"` so screen readers do not announce the "|" character.
- The `body::after` watermark pseudo-element must not create z-index or compositing conflicts with foreground glass cards.
- The "meet the founder" nav and hero links both target an external URL — `rel="noopener noreferrer"` is required on both to prevent tab-nabbing.
- At `@media (max-width: 768px)`, the watermark layer behavior (scale vs. hide) must be defined explicitly to avoid layout artifacts.

---

## Requirements _(mandatory)_

### Functional Requirements

#### REQ-0: Constitutional Pre-Condition

- **FR-001**: `constitution.md` MUST be amended to v2.1.0 (MINOR bump). Principle IV MUST be updated to permit external attribution link(s) in the navigation alongside the three required in-page anchor links. This task MUST be completed and committed before any `index.html` or `index.css` changes begin.

#### REQ-1: Nav Logo

- **FR-002**: The `.nav-brand` element's text content (`Metamatopoeia`) MUST be replaced with `<img src="./assets/Metamatopoeia_simple_small.png" alt="Metamatopoeia" class="nav-logo" />`.
- **FR-003**: A `.nav-logo` CSS rule MUST be added: `display: block; height: 36px; width: auto;` (height in the 32px–40px range; exact value subject to visual review during clarification).
- **FR-004**: The existing `.nav-brand` CSS rule MUST NOT be removed; it retains its layout and flex properties. Only the inner HTML content changes.

#### REQ-2: Hero Asymmetric Dual-Text CTA

- **FR-005**: The `<a class="button button-primary" href="#workshop" ...>Explore Workshop</a>` element MUST be removed from the hero card in `index.html`.
- **FR-006**: A `<div class="hero-cta">` element MUST be added inside `.hero-card`, immediately after `<p class="hero-tagline">`, containing exactly:
  1. `<a href="#workshop" class="hero-cta-link" aria-label="Scroll to Workshop section">Scroll to Workshop</a>`
  2. `<span class="hero-cta-divider" aria-hidden="true">|</span>`
  3. `<a href="https://sds-smith.io" class="hero-cta-link" target="_blank" rel="noopener noreferrer" aria-label="Meet the Founder, visit personal website">Meet the Founder</a>`
- **FR-007**: A `.hero-cta` CSS rule MUST be added: `display: flex; align-items: center; gap: 1rem; margin-top: 1.5rem; flex-wrap: wrap;`
- **FR-008**: A `.hero-cta-link` CSS rule MUST be added with no background, no border, and no padding that creates a button silhouette. Color: `var(--lg-color-text-muted)` default, transitioning to `var(--lg-color-text-primary)` on `:hover`/`:focus`. Font-size: `var(--lg-typography-size-sm)`. `text-decoration: none` in default state.
- **FR-009**: A `.hero-cta-divider` CSS rule MUST be added: `color: var(--lg-color-text-muted); opacity: 0.4; user-select: none;`

#### REQ-3: Nav "Meet the Founder" Link

- **FR-010**: A fourth `<li>` MUST be inserted into `.nav-links` between the "workshop" `<li>` and the "contact" `<li>`, containing: `<a href="https://sds-smith.io" class="nav-link" target="_blank" rel="noopener noreferrer" aria-label="Meet the Founder, visit personal website">meet the founder</a>`
- **FR-011**: The existing `.nav-link` CSS rule applies to this element without modification. No new class is required.

#### REQ-4: Background Logo Watermark

- **FR-012**: A `body::after` pseudo-element MUST be added in `index.css` with: `content: ""; position: fixed; top: 0; left: 0; right: 0; bottom: 0; z-index: -1; pointer-events: none;` — using `body::after` because CSS `background` shorthand does not support per-layer opacity, and the watermark requires an explicit `opacity` value distinct from `body::before`.
- **FR-013**: `body::after` MUST set `background: url("./assets/Metamatopoeia_simple_transparent.png") left center no-repeat; background-size: auto 55vh;` (exact size subject to visual review).
- **FR-014**: `body::after` MUST set `opacity: 0.45` to achieve the "present but secondary" medium-opacity treatment (~40–50% range).
- **FR-015**: The watermark MUST remain a CSS pseudo-element background image — not an `<img>` element — so it is inherently excluded from the accessibility tree.
- **FR-016**: At `@media (max-width: 768px)`, `body::after` MUST set `display: none` or `opacity: 0` to suppress the watermark and avoid competing with content on small screens.

#### Carry-Forward Constitutional Requirements

- **FR-017**: All color values MUST derive from the four palette tokens: `--color-slate`, `--color-teal`, `--color-mist`, `--color-frost`.
- **FR-018**: The JavaScript `<script>` block MUST remain ≤30 non-empty source lines (no JS changes required by this feature).
- **FR-019**: Zero external network requests at page load. `https://sds-smith.io` is a user-activated hyperlink target, not a resource fetched by the page — this does not violate the constraint.
- **FR-020**: Semantic HTML5 elements, `clamp()` typography, mobile-first breakpoints, and ARIA labels MUST all be preserved.

---

### Key Entities

- **`.nav-logo`**: `<img>` inside `.nav-brand`; renders the diamond mark at a fixed height in the header.
- **`.hero-cta`**: Flex row container inside the hero card housing the asymmetric dual text links and divider.
- **`.hero-cta-link`**: Typographic anchor — no button shape, styled as inline text with a hover/focus color transition.
- **`.hero-cta-divider`**: Decorative `<span>` separator, `aria-hidden="true"`, low-opacity muted color.
- **`body::after`**: New pseudo-element dedicated to the watermark; `position: fixed`, `opacity: 0.45`, purely decorative, excluded from the accessibility tree.

---

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: The nav-brand area renders the logo image; the text string "Metamatopoeia" is no longer a visible text node in the header. Verified by DevTools DOM inspection.
- **SC-002**: The hero card contains no element with class `.button` or `.button-primary`. Verified by DevTools DOM search.
- **SC-003**: The hero CTA area renders exactly two text links and one `aria-hidden` divider below `.hero-tagline`. Both links are individually Tab-focusable. Verified by visual inspection and keyboard navigation.
- **SC-004**: Clicking "Meet the Founder" (hero) and "meet the founder" (nav) each open `https://sds-smith.io` in a new browser tab. Verified by manual click test.
- **SC-005**: The nav renders four links in order: home, workshop, meet the founder, contact. Verified by visual inspection.
- **SC-006**: At ≥1024px viewport, the logo watermark is visible in the left background area and does not overlap the foreground glass card. Verified by visual inspection.
- **SC-007**: DevTools Network tab shows zero external requests on cold page load (cache disabled).
- **SC-008**: `index.html` opens without console errors from the filesystem.
- **SC-009**: The JavaScript `<script>` block remains ≤30 non-empty source lines.
- **SC-010**: The page renders without horizontal overflow at 280px–1440px viewport widths with the four-link nav.
- **SC-011**: `constitution.md` version reads `2.1.0` before any implementation file is touched.

---

## Assumptions

- `Metamatopoeia_simple_transparent.png` has a true transparent background suitable for compositing over the existing gradient overlay without a white-fill artifact.
- `Metamatopoeia_simple_small.png` has an opaque slate-colored background; this is intentional and the visual treatment is acceptable at the target nav-logo height (~36px).
- `https://sds-smith.io` is a live, stable URL at implementation time; the spec does not validate availability.
- No new JavaScript is required. All feature changes are HTML structure and CSS only.
- The `body::after` pseudo-element can share the fixed negative z-index background plane with `body::before` without compatibility issues.
- The Principle IV amendment (v2.1.0) will be authored and committed before any `index.html` or `index.css` changes begin.
- Existing CSS palette tokens (`--color-slate-rgb`, `--lg-color-text-muted`, `--lg-color-text-primary`, etc.) are already declared in `:root` and available to all new rules.
