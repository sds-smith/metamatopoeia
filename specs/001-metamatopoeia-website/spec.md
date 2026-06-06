# Feature Specification: Metamatopoeia Website

**Feature Branch**: `001-metamatopoeia-website`  
**Created**: 2026-06-05  
**Status**: Clarified  
**Input**: Build the Metamatopoeia brand website — a single-page software project showcase mirroring the structure, CSS architecture, and liquid glass design system of `sds-smith/html_portfolio`, remapped to the Metamatopoeia brand, palette, and constitution constraints.

---

## User Scenarios & Testing _(mandatory)_

### User Story 1 — First-Time Visitor Orientation (Priority: P1)

A visitor lands on the site for the first time and immediately understands what Metamatopoeia is, can navigate to see projects, and can reach out via a contact channel — all without leaving the page or triggering a network request.

**Why this priority**: Core brand entry point. Every other story depends on this working correctly.

**Independent Test**: Open `index.html` directly from the filesystem (no server). The page loads completely, all three sections are visible, navigation links scroll to the correct anchors, and all contact links resolve to valid `mailto:` or external URLs.

**Acceptance Scenarios**:

1. **Given** a visitor opens `index.html` from the filesystem, **When** the page loads, **Then** the Hero section is visible first with the Metamatopoeia brand name prominent in the header, a headline/tagline card, and a CTA linking to `#workshop`.
2. **Given** the visitor clicks the "workshop" nav link, **When** the scroll completes, **Then** the Workshop section is fully in view showing project cards.
3. **Given** the visitor clicks the "contact" nav link, **When** the scroll completes, **Then** the Contact section is in view showing all three contact channels.
4. **Given** the page is loaded, **When** the visitor inspects the network tab, **Then** zero external network requests are made (no fonts, no CDN, no analytics, no remote images).

---

### User Story 2 — Project Discovery in Workshop (Priority: P2)

A visitor browses the Workshop section to evaluate Metamatopoeia's project portfolio, reading project descriptions and following links to source code or live demos.

**Why this priority**: Primary purpose of the site — showcasing projects. Directly communicates the brand's technical depth.

**Independent Test**: Navigate directly to `index.html#workshop`. All four project cards render with title, description, and at least one action button with a valid `href`.

**Acceptance Scenarios**:

1. **Given** the visitor views the Workshop section, **When** the section renders, **Then** exactly four project cards are displayed: Cup, Liquid Glass UI, Discover Breweries, and Assemble the Jams.
2. **Given** a project card is rendered, **When** the visitor reads it, **Then** each card shows: project title, short description, and one or more action buttons.
3. **Given** the visitor clicks a "View Source Code" button, **When** the link resolves, **Then** it opens the correct GitHub repository in a new tab.
4. **Given** the visitor is on mobile (<768px), **When** the Workshop section renders, **Then** cards stack vertically at full width with legible text and tappable buttons (min 44px tap targets).

---

### User Story 3 — Contact via SpeedDial FAB (Priority: P3)

A visitor uses the persistent floating SpeedDial FAB to quickly access any of the three Metamatopoeia contact channels from anywhere on the page.

**Why this priority**: Provides persistent, page-agnostic contact access, mirroring the reference's primary contact pattern.

**Independent Test**: With the page open, click the FAB. The three contact action buttons expand. Click each one and verify the correct destination. Press Escape — the FAB closes. Click outside the FAB — it closes.

**Acceptance Scenarios**:

1. **Given** the FAB is closed, **When** the visitor clicks the FAB, **Then** three action buttons expand (Email, GitHub, LinkedIn) with appropriate SVG icons and ARIA labels.
2. **Given** the FAB is open, **When** the visitor clicks the Email action, **Then** the browser opens `mailto:metamatopoeia@gmail.com` and the FAB closes.
3. **Given** the FAB is open, **When** the visitor clicks outside the speed-dial container, **Then** the FAB closes without navigation.
4. **Given** the FAB is open, **When** the visitor presses `Escape`, **Then** the FAB closes.
5. **Given** a keyboard-only user, **When** they tab to the FAB and press `Enter`, **Then** the actions expand and each action is reachable by subsequent Tab presses.

---

### User Story 4 — Contact Section (Priority: P3)

A visitor scrolls to or navigates to the Contact section and finds all three contact channels explicitly listed with clear labels and clickable links.

**Why this priority**: Complements the FAB with an in-page, fully discoverable contact surface that's accessible without the floating component.

**Independent Test**: Navigate to `index.html#contact`. All three contact channels (email, GitHub, LinkedIn) are visible, labeled, and their links resolve correctly.

**Acceptance Scenarios**:

1. **Given** the visitor navigates to the Contact section, **When** the section renders, **Then** all three channels are displayed: Email (`metamatopoeia@gmail.com`), GitHub (`github.com/metamatopoeia`), LinkedIn (`linkedin.com/company/metamatopoeia`).
2. **Given** a channel link is clicked, **When** the browser resolves the link, **Then** email opens `mailto:`, GitHub and LinkedIn open in a new tab with `rel="noopener noreferrer"`.

---

### User Story 5 — Accessibility & Reduced-Motion (Priority: P2)

A visitor using assistive technology or a device with `prefers-reduced-motion` or `prefers-reduced-transparency` enabled experiences a fully usable, readable site.

**Why this priority**: Non-negotiable quality gate per the constitution.

**Independent Test**: Enable OS-level reduced motion. Reload page — all CSS transitions are disabled. Enable high-contrast/reduce-transparency — glass blur and opacity fall back gracefully. Tab through the entire page without a mouse — all interactive elements are reachable and visible.

**Acceptance Scenarios**:

1. **Given** `prefers-reduced-motion: reduce` is active, **When** the page loads, **Then** `--lg-glass-transition-duration` is `0ms` and `scroll-behavior` is `auto`.
2. **Given** `prefers-reduced-transparency: reduce` is active, **When** the page loads, **Then** `--lg-glass-bg-opacity` is `0.98` and `backdrop-filter` is `none`.
3. **Given** a screen reader user, **When** they navigate the page, **Then** all decorative elements have `aria-hidden="true"` and all interactive elements have accessible labels.
4. **Given** a keyboard-only user, **When** they tab through the page, **Then** focus indicators are visible on all interactive elements.

---

### Edge Cases

- What happens when a project image asset is missing? Cards must still render with graceful `alt` text fallback — no broken layout.
- What happens on a very narrow viewport (<320px)? Layout must not overflow horizontally.
- What happens if the visitor has both `prefers-reduced-motion` and `prefers-reduced-transparency` active simultaneously? Both overrides apply without conflict.
- What happens if `backdrop-filter` is unsupported (older browsers)? The `--lg-glass-bg-opacity` opaque fallback makes cards readable.

---

## Requirements _(mandatory)_

### Functional Requirements

- **FR-001**: The deliverable MUST be a single `index.html` file paired with a single `index.css` file, openable directly from the filesystem without a build step or server.
- **FR-002**: The page MUST contain exactly three top-level content sections in order: Hero (`id="hero"`), Workshop (`id="workshop"`), Contact (`id="contact"`).
- **FR-003**: The sticky header navigation MUST contain three links anchoring to `#hero`, `#workshop`, and `#contact`, labeled "home", "workshop", and "contact" respectively.
- **FR-004**: The header MUST display the brand name "Metamatopoeia" — no personal names anywhere in markup or content.
- **FR-005**: The Hero section MUST contain a `.card-elevated` glass card with the tagline **"Human-Centered Software Intentionally Designed"** and a CTA button linking to `#workshop`.
- **FR-006**: The Workshop section MUST contain exactly four `.card-elevated` project cards: Cup, Liquid Glass UI, Discover Breweries, and Assemble the Jams.
- **FR-007**: Each project card MUST display: project title, description, and action button(s) linking to the project's GitHub repository. Characterization labels (Open-Source / Home Lab / Experimental) are deferred to a future iteration.
- **FR-008**: The Contact section MUST use a `.card-elevated` glass card layout to explicitly display all three authorized contact channels: `metamatopoeia@gmail.com`, `https://github.com/metamatopoeia`, `https://www.linkedin.com/company/metamatopoeia`.
- **FR-009**: A SpeedDial FAB MUST be fixed-positioned (bottom-right) and persistent across all scroll positions, exposing the same three contact channels (Email, GitHub, LinkedIn) on expand.
- **FR-010**: The SpeedDial FAB MUST be controlled by no more than 30 non-empty JavaScript source lines inside the single `<script>` block, excluding the `<script>` tags themselves. JavaScript may only handle FAB toggle, close on action click, close on outside click, and close on Escape key.
- **FR-011**: All color values in `index.css` MUST derive exclusively from the four Metamatopoeia palette tokens: `--color-slate: #5A606A`, `--color-teal: #79A1A2`, `--color-mist: #BDBFC6`, `--color-frost: #EFF1F3`. No other hex, rgb, hsl, or named color values permitted.
- **FR-012**: All Liquid Glass design tokens MUST follow the `--lg-{group}-{subgroup}-{token}` naming convention.
- **FR-013**: Dark mode MUST be handled via `prefers-color-scheme: dark` media query overriding CSS custom properties only.
- **FR-014**: Reduced-motion MUST be handled via `prefers-reduced-motion: reduce` setting `--lg-glass-transition-duration: 0ms` and `scroll-behavior: auto`.
- **FR-015**: Reduced-transparency MUST be handled via `prefers-reduced-transparency: reduce` setting `--lg-glass-bg-opacity: 0.98` and `backdrop-filter: none`.
- **FR-016**: The background MUST use the local asset `./assets/background-image-profile.jpeg` with a token-derived alpha overlay, sourced from the reference repository.
- **FR-017**: All card media images MUST be local assets sourced from the reference repository's `public/assets/` directory.
- **FR-018**: Zero external network requests are permitted — no CDN resources, web fonts, remote images, or analytics scripts.
- **FR-019**: Semantic HTML5 elements MUST be used where structurally appropriate, including `<header>`, `<nav>`, `<main>`, `<section>`, and `<article>`. A `<footer>` is not required for v1 unless footer-specific content is introduced without adding a fourth top-level content section.
- **FR-020**: All decorative elements MUST carry `aria-hidden="true"`. All interactive elements MUST have appropriate ARIA labels.
- **FR-021**: Font sizing MUST use `clamp()` for fluid typography scaling.
- **FR-022**: Layout MUST be mobile-first with a primary breakpoint at 768px.
- **FR-023**: The `<title>`, `<meta name="description">`, and Open Graph tags MUST reference Metamatopoeia and describe it as a software project showcase.
- **FR-024**: No JavaScript frameworks, build tools, CSS preprocessors, or `<iframe>` embeds are permitted.
- **FR-025**: The AboutApp popover component from the reference MUST NOT be included (contains personal copyright attribution).

### Key Entities

- **Project Card**: Represents a showcased software project. Attributes: title, description, media image, action button(s) with href. (Characterization labels deferred to v2.)
- **Contact Channel**: An authorized outreach link. Exactly three exist: Email, GitHub, LinkedIn — each with an SVG icon and ARIA label.
- **SpeedDial FAB**: A floating action button that expands to reveal Contact Channels. State: open/closed, managed by checkbox hack plus no more than 30 non-empty JavaScript source lines.

---

## Success Criteria _(mandatory)_

### Measurable Outcomes

- **SC-001**: `index.html` opens and renders completely in any modern browser directly from the filesystem (no server required, no console errors).
- **SC-002**: All three navigation anchors scroll to their respective sections without JavaScript (CSS `scroll-behavior: smooth`).
- **SC-003**: Zero external network requests are recorded in the browser DevTools Network tab on a cold load.
- **SC-004**: The SpeedDial FAB JavaScript block contains no more than 30 non-empty JavaScript source lines, excluding `<script>` tags and blank lines.
- **SC-005**: All four project cards render correctly with images, descriptions, and working action buttons (no characterization labels in v1).
- **SC-006**: With `prefers-reduced-motion` enabled, no CSS transitions or animations execute.
- **SC-007**: The page passes keyboard-only navigation — every interactive element is reachable via Tab and activatable via Enter/Space.
- **SC-008**: No personal name or biographical copy appears in visible text, metadata, headings, labels, or attribution in `index.html` or `index.css`; canonical external repository URL paths are permitted when required by project links.
- **SC-009**: Every color value in `index.css` traces back to one of the four Metamatopoeia palette tokens.
- **SC-010**: The page renders without horizontal overflow at viewport widths from 280px to 1440px.

---

## Assumptions

- The background image (`background-image-profile.jpeg`) and all card media images will be copied from the reference repository (`sds-smith/html_portfolio`) into `./assets/` during the implementation phase.
- The Cup project card mirrors the reference exactly with three action buttons: "Check out the Gist" (links to the Gist), "Check out the Preview" (links to `cupsocial.app`), and "Join the Beta" (links to `cupsocial.app`).
- The Liquid Glass UI repo is at `github.com/metamatopoeia/liquid-glass-ui`.
- Discover Breweries links to `github.com/sds-smith/discover-breweries`. Assemble the Jams links to `github.com/sds-smith/assemble_the_jams_3`.
- No favicon asset is assumed to exist yet; a simple inline SVG favicon or omission is acceptable for v1.
- The page will use normal document scrolling with section scroll margins; the reference `.scrollable` container pattern is not required for v1 unless explicitly added to the HTML/CSS contracts and task list.
- Dark mode support is included per the constitution but the primary design is light-mode with the Metamatopoeia palette.
