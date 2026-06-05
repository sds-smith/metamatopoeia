<!--
  SYNC IMPACT REPORT
  ==================
  Version change: 1.0.0 → 1.0.1 (PATCH — wording clarification in Brand & Content Standards)

  Modified principles: N/A

  Added sections: N/A

  Removed sections: N/A

  Templates reviewed:
    ✅ .specify/templates/plan-template.md       — no changes required
    ✅ .specify/templates/spec-template.md       — no changes required
    ✅ .specify/templates/tasks-template.md      — no changes required
    ✅ .specify/templates/checklist-template.md  — no changes required

  Follow-up TODOs: none
-->

# Metamatopoeia Constitution

## Core Principles

### I. Zero-Dependency, Build-Free

The deliverable MUST consist solely of vanilla HTML, CSS, and at most ~30 lines of vanilla
JavaScript. Build tools (webpack, Vite, Parcel, etc.), CSS preprocessors (Sass, Less, PostCSS),
and JavaScript frameworks or libraries (React, Vue, Alpine, etc.) are PROHIBITED. No external
CDN resources, web fonts, or network requests of any kind are permitted. The site MUST open
directly from the filesystem as a static file without any build step.

**Rationale**: Mirrors the zero-dependency philosophy of the reference `html_portfolio`;
ensures maximum longevity, portability, and instant load performance with zero infrastructure
overhead.

### II. Liquid Glass Design System

All visual effects — glassmorphism surfaces, backdrop blur, gradient reflections, layered
box-shadows, and smooth transitions — MUST be expressed exclusively through CSS. Design tokens
MUST be declared as CSS custom properties. The naming convention MUST follow
`--lg-{group}-{subgroup}-{token}` (e.g., `--lg-glass-blur`, `--lg-glass-bg-opacity`).
Inline styles for design tokens are PROHIBITED. Dark-mode and reduced-motion variants MUST be
handled via `prefers-color-scheme` and `prefers-reduced-motion` media queries within the
single CSS file.

**Rationale**: Keeps the design system auditable, themeable, and fully decoupled from markup
structure. Guarantees visual consistency across all sections without runtime JavaScript.

### III. Metamatopoeia Color Palette (NON-NEGOTIABLE)

The authorized palette is exactly four colors:

| Token           | Hex       | Role                 |
| --------------- | --------- | -------------------- |
| `--color-slate` | `#5A606A` | Primary dark / text  |
| `--color-teal`  | `#79A1A2` | Accent / interactive |
| `--color-mist`  | `#BDBFC6` | Mid-tone / borders   |
| `--color-frost` | `#EFF1F3` | Light surface / bg   |

All color values used anywhere in the CSS MUST derive from these four tokens via CSS custom
properties. No other hex, rgb, hsl, or named color values are permitted. Alpha variants
(e.g., `rgba(var(--color-teal-rgb), 0.12)`) MUST still trace back to a palette token.

**Rationale**: Enforces strict brand identity for Metamatopoeia across all present and future
pages or components.

### IV. Single-Page, Three-Section Architecture

The deliverable MUST be a single `index.html` file. The page MUST contain exactly three
top-level content sections in this order: **Hero**, **Workshop**, **Contact**. No About
section exists. Navigation MUST reference only these three in-page anchors. No multi-page
routing, `<iframe>` embeds, or JavaScript-driven page transitions are permitted.

**Rationale**: Reduces cognitive overhead for visitors; keeps the site focused on showcasing
projects rather than personal biography. Single-file delivery aligns with the zero-dependency
principle.

### V. Accessibility & Performance

- Semantic HTML5 elements (`<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`, etc.)
  MUST be used throughout. Decorative elements MUST use `aria-hidden="true"`.
- All interactive elements MUST be keyboard-navigable and carry appropriate ARIA labels where
  native semantics are insufficient.
- Responsive layouts MUST be mobile-first; breakpoints defined via CSS custom properties or
  named media queries.
- `clamp()` MUST be used for fluid typography scaling.
- Zero external network requests is a hard constraint (no Google Fonts, no analytics scripts,
  no remote images).

**Rationale**: Accessibility and performance are non-negotiable quality gates, not
afterthoughts. A static site with zero dependencies has no excuse for poor scores on either
dimension.

## Brand & Content Standards

- All branding MUST use **Metamatopoeia** exclusively. Personal names (e.g., "Shawn Smith")
  MUST NOT appear anywhere in markup, metadata, or copy.
- The three and only authorized contact channels are:
  - **Email**: metamatopoeia@gmail.com
  - **GitHub**: https://github.com/metamatopoeia
  - **LinkedIn**: https://www.linkedin.com/company/metamatopoeia
- Projects in the Workshop section MAY be characterized as **Open-Source**, **Paid Client**,
  and/or **Home Lab / Experimental**. These are descriptive, non-exclusive labels — a project
  may carry more than one characterization or none at all. They exist to give visitors context,
  not to enforce a rigid taxonomy.
- No biographical, personal-identity, or "About Me" content is permitted anywhere in the site.
- The `<title>`, `<meta name="description">`, and Open Graph tags MUST reference Metamatopoeia
  and accurately reflect its purpose as a software project showcase brand.

## Development Constraints

- A single `index.css` file MUST serve as the sole stylesheet for `index.html`.
- JavaScript MUST be vanilla, contained within a single `<script>` block at the bottom of
  `<body>`, and MUST NOT exceed ~30 lines. No separate `.js` files.
- No server-side rendering, no templating engines, no static site generators. The output is
  pure hand-authored (or AI-authored) HTML and CSS.
- All spec, plan, and task artifacts MUST be validated against the Constitution Check gate in
  `plan.md` before implementation begins. Any deviation from these constraints MUST be
  documented in a Complexity Tracking table with justification.
- Amendments to this constitution MUST follow the semantic versioning policy defined in the
  Governance section and be recorded in a new Sync Impact Report comment at the top of this
  file.

## Governance

This constitution supersedes all other project guidance. In cases of conflict between this
document and any spec, plan, task list, or checklist, this constitution takes precedence.

**Amendment procedure**:

1. Identify the principle(s) affected and the version bump type (MAJOR/MINOR/PATCH).
2. Draft the updated constitution content and Sync Impact Report.
3. Propagate changes to all dependent templates and artifacts.
4. Overwrite `.specify/memory/constitution.md` with the amended version.
5. Stage and commit via the user's terminal with a message of the form
   `docs: amend constitution to vX.Y.Z (<summary>)`.

**Versioning policy**:

- MAJOR: Removal or incompatible redefinition of an existing principle.
- MINOR: New principle, new section, or materially expanded guidance added.
- PATCH: Clarifications, wording refinements, typo fixes.

**Compliance review**: Every `/speckit.plan` invocation MUST complete the Constitution Check
gate before Phase 0 research proceeds. Every `/speckit.tasks` output MUST not introduce tasks
that violate these principles without documented justification.

**Version**: 1.0.1 | **Ratified**: 2026-06-05 | **Last Amended**: 2026-06-05
