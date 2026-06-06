# Implementation Plan: Metamatopoeia Website

**Branch**: `001-metamatopoeia-website` | **Date**: 2026-06-05 | **Spec**: [spec.md](./spec.md)
**Input**: Feature specification from `/specs/001-metamatopoeia-website/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/plan-template.md` for the execution workflow.

## Summary

Build a single-page static website showcasing Metamatopoeia software projects. The deliverable consists of `index.html` and `index.css` (plus ≤30 non-empty source lines of vanilla JS for the SpeedDial FAB), openable directly from the filesystem without a build step. The design mirrors the structure and Liquid Glass CSS architecture of the reference repository `sds-smith/html_portfolio`, remapped to the Metamatopoeia brand palette and constitution constraints. The page contains three sections (Hero, Workshop, Contact) with a sticky header navigation and a persistent floating SpeedDial FAB for contact channels.

## Technical Context

<!--
  ACTION REQUIRED: Replace the content in this section with the technical details
  for the project. The structure here is presented in advisory capacity to guide
  the iteration process.
-->

**Language/Version**: HTML5, CSS3, Vanilla JavaScript (ES6+)
**Primary Dependencies**: None (zero-dependency requirement per constitution)
**Storage**: N/A (static files only)
**Testing**: Manual browser testing (no automated test framework per constitution)
**Target Platform**: Modern browsers (Chrome, Firefox, Safari, Edge) - filesystem-openable static files
**Project Type**: static-website
**Performance Goals**: Zero external network requests, filesystem-openable rendering, no external render-blocking resources, no console errors on cold load
**Constraints**: ≤30 non-empty JavaScript source lines, no build tools, no external CDN resources, no frameworks, single HTML + CSS file
**Scale/Scope**: Single page, 3 sections, 4 project cards, 3 contact channels

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

### Principle I: Zero-Dependency, Build-Free

- ✅ **PASS**: Spec requires single `index.html` + `index.css` with ≤30 non-empty source lines of vanilla JS
- ✅ **PASS**: FR-018 explicitly prohibits external network requests, CDN resources, web fonts
- ✅ **PASS**: FR-024 prohibits JavaScript frameworks, build tools, CSS preprocessors
- ✅ **PASS**: FR-001 requires filesystem-openable delivery without build step

### Principle II: Liquid Glass Design System

- ✅ **PASS**: FR-012 requires `--lg-{group}-{subgroup}-{token}` naming convention
- ✅ **PASS**: FR-013-015 handle dark mode, reduced-motion, reduced-transparency via media queries
- ✅ **PASS**: Constitution requires CSS custom properties for all design tokens

### Principle III: Metamatopoeia Color Palette

- ✅ **PASS**: FR-011 restricts all colors to the four palette tokens (slate, teal, mist, frost)
- ✅ **PASS**: Constitution explicitly prohibits any other hex, rgb, hsl, or named color values

### Principle IV: Single-Page, Three-Section Architecture

- ✅ **PASS**: FR-002 mandates exactly three sections: Hero, Workshop, Contact
- ✅ **PASS**: FR-003 requires navigation anchors to these three sections only
- ✅ **PASS**: FR-025 explicitly excludes About section (personal content)
- ✅ **PASS**: FR-019 requires semantic HTML5 elements

### Principle V: Accessibility & Performance

- ✅ **PASS**: FR-020 requires ARIA labels and `aria-hidden` for decorative elements
- ✅ **PASS**: FR-021 requires `clamp()` for fluid typography
- ✅ **PASS**: FR-022 requires mobile-first responsive layout
- ✅ **PASS**: FR-018 enforces zero external network requests
- ✅ **PASS**: User Story 5 covers reduced-motion and reduced-transparency support

### Brand & Content Standards

- ✅ **PASS**: FR-004 requires brand name "Metamatopoeia" only, no personal names
- ✅ **PASS**: FR-008 authorizes exactly three contact channels (email, GitHub, LinkedIn)
- ✅ **PASS**: FR-023 requires Metamatopoeia metadata in title and description tags
- ✅ **PASS**: FR-007 defers characterization labels to future iteration (permitted flexibility)

### Development Constraints

- ✅ **PASS**: FR-001 requires single `index.css` file
- ✅ **PASS**: FR-010 limits JavaScript to ≤30 non-empty source lines
- ✅ **PASS**: Constitution requires spec/plan/task validation against Constitution Check

**GATE STATUS**: ✅ **PASS** - No violations detected. All constitution principles are satisfied by the specification.

---

## Post-Design Constitution Re-Check

_GATE: Re-evaluated after Phase 1 design artifacts generation._

### Design Artifact Compliance Review

#### HTML Structure Contract (contracts/html-structure.md)

- ✅ **PASS**: Single `index.html` with semantic HTML5 elements
- ✅ **PASS**: Exactly three sections (Hero, Workshop, Contact)
- ✅ **PASS**: No personal names in markup
- ✅ **PASS**: Exactly three contact channels
- ✅ **PASS**: All interactive elements have ARIA labels
- ✅ **PASS**: External links have `rel="noopener noreferrer"`
- ✅ **PASS**: JavaScript limited to ≤30 non-empty source lines for SpeedDial FAB
- ✅ **PASS**: No external CDN resources or network requests

#### CSS Structure Contract (contracts/css-structure.md)

- ✅ **PASS**: All colors derive from four palette tokens (slate, teal, mist, frost)
- ✅ **PASS**: Liquid Glass tokens follow `--lg-{group}-{subgroup}-{token}` naming
- ✅ **PASS**: Dark mode via `prefers-color-scheme` media query
- ✅ **PASS**: Reduced motion via `prefers-reduced-motion` media query
- ✅ **PASS**: Reduced transparency via `prefers-reduced-transparency` media query
- ✅ **PASS**: Typography uses `clamp()` for fluid scaling
- ✅ **PASS**: Mobile-first responsive design with 768px breakpoint
- ✅ **PASS**: No hardcoded colors, no named colors, no inline styles for tokens

#### Data Model (data-model.md)

- ✅ **PASS**: Exactly four project cards (Cup, Liquid Glass UI, Discover Breweries, Assemble the Jams)
- ✅ **PASS**: Exactly three contact channels (Email, GitHub, LinkedIn)
- ✅ **PASS**: Characterization labels deferred to v2 (permitted flexibility)
- ✅ **PASS**: All design tokens follow naming convention
- ✅ **PASS**: Media query overrides documented for accessibility

#### Quickstart (quickstart.md)

- ✅ **PASS**: Zero-dependency implementation steps
- ✅ **PASS**: Color palette constraints emphasized
- ✅ **PASS**: JavaScript limit emphasized
- ✅ **PASS**: Constitution compliance checklist included

### Design-Specific Compliance Notes

**Color Implementation**: The CSS contract explicitly prohibits hardcoded colors and enforces the four-token palette. All color usage patterns are documented as allowed (direct token, semantic token, alpha color with palette channel token, opacity variants) or prohibited (hardcoded hex, named colors, RGB without token).

**Design Token Naming**: The CSS contract enforces the `--lg-{group}-{subgroup}-{token}` convention and explicitly prohibits inline styles for design tokens.

**JavaScript Scope**: The HTML contract limits JavaScript to a single ≤30 non-empty source line block for SpeedDial FAB functionality only, with no frameworks or separate `.js` files.

**Asset Strategy**: The quickstart documents that all assets must be local (copied from reference repository), ensuring zero external network requests.

**POST-DESIGN GATE STATUS**: ✅ **PASS** - All design artifacts comply with constitution principles. No violations introduced during Phase 1 design.

## Project Structure

### Documentation (this feature)

```text
specs/[###-feature]/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
index.html              # Single-page HTML document
index.css              # Single stylesheet with Liquid Glass design system
assets/
├── background-image-profile.jpeg    # Background image from reference
└── [project-card-images]/           # Card media images from reference
```

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

No constitution violations detected. This section remains empty.
