# Context Snapshot — 003-branding-and-founder-bridge

**Generated**: 2026-06-06
**Phase**: 4 (Clarify) complete — ready for Phase 5 handoff (SWE-1.6, new session)

---

## Purpose

This file captures all conversational nuance, design decisions, and architectural constraints from Phases 2–4 that are not already encoded in `spec.md`. A fresh model can restore full context from `constitution.md` + `spec.md` + this file alone.

---

## Feature Identity

- **Branch**: `003-branding-and-founder-bridge`
- **Predecessor**: `002-layout-refinement` (completed; memory purged)
- **Constitution version at spec time**: `2.0.0` (v2.1.0 amendment is the first task)

---

## Discovery Decisions (Phase 2 nuance)

### Logo Assets
Two PNG assets were added to `assets/` during the discovery session:

| File | Size | Use |
|---|---|---|
| `Metamatopoeia_simple_small.png` | 38KB | Nav brand mark |
| `Metamatopoeia_simple_transparent.png` | 3.2MB | Background watermark |

**Visual description of the mark**: A geometric diamond/chevron-style icon in brand teal (`--color-teal`), composed of nested concentric diamond shapes with a small teal square at the center.

**`_small.png` background**: Has an **opaque slate-colored background** (matching `--color-slate` #5A606A). This is intentional — the logo will appear as a branded square stamp in the nav. The user explicitly chose this asset for the nav and accepted the opaque background treatment.

**`_transparent.png` background**: True transparent background. The logo mark itself renders at natural teal opacity. The opaque-area pixels are the teal mark only.

### Hero CTA — Asymmetric Dual-Text
The user described the pattern explicitly as:
> "Asymmetric Dual-Text (The Minimalist Route) — Remove the button shape entirely and use elegant, typographic hierarchy."

- Exact link text: `"Scroll to Workshop"` | `"Meet the Founder"` — these are the verbatim approved strings
- Separated by a minimal `|` divider character in a `<span aria-hidden="true">`
- No button shape, no padding that creates a button silhouette — pure typographic `<a>` elements

### Nav Structure
User explicitly specified the new nav order:
> "home / workshop / about the founder / contact"

Then refined the label to `"meet the founder"` (matching the hero CTA label). The nav link uses **lowercase** to match the existing nav-link convention (`home`, `workshop`, `contact` are all lowercase).

### Personal Website URL
`https://sds-smith.io` — confirmed, verbatim. Used in both hero CTA and nav link.

---

## Architectural Decisions (Phase 4)

### Watermark: body::after, not body::before extra layer
**Decision**: Use a new `body::after` pseudo-element for the watermark, not an additional layer in `body::before`.

**Reason**: CSS `background` shorthand does not support per-layer `opacity`. To achieve the "medium opacity — present but secondary" treatment (~40–50%), an independent pseudo-element with `opacity: 0.45` is required.

`body::before` is already used for the page background (profile image + overlay gradient). `body::after` is currently unused and is the clean, minimal solution.

**Approved opacity**: `0.45` (tunable ±0.05 during visual review in Phase 7).

### Watermark Positioning
- `background-position: left center` — left side of the viewport where profile image is absent
- `background-size: auto 55vh` — large enough to be a meaningful brand presence, small enough not to overwhelm
- Both values are marked "subject to visual review" — the planner should treat `55vh` as a starting value, not a hard pixel requirement

### body::after z-index
Must use `z-index: -1` to sit behind all page content (same as `body::before`). Both pseudo-elements occupy the same stacking layer beneath content.

### No HTML changes for watermark
The watermark is 100% CSS — no new `<img>`, `<div>`, or any HTML element. This satisfies the constitution's accessibility requirement (CSS background images are not in the accessibility tree) and keeps the HTML clean.

---

## Constitutional Impact

### Amendment Required: Principle IV → v2.1.0 (MINOR)

**Current text (v2.0.0)**:
> "Navigation MUST reference only these three in-page anchors."

**Required change**: Add a clause permitting external attribution links in the nav alongside the three required in-page anchors.

**Suggested amended text**:
> "Navigation MUST reference the three required in-page anchors (Hero, Workshop, Contact). Additional external attribution links MAY appear in the navigation at the author's discretion."

**Governance**: This is a MINOR bump (new permissive guidance added, no principle removed or incompatibly redefined). The amendment must be:
1. Authored with a Sync Impact Report comment block at the top of `constitution.md`
2. Version bumped to `2.1.0`
3. Committed before any `index.html` or `index.css` edits begin (SC-011)

---

## Files to Touch (Implementation Scope)

| File | Change Type |
|---|---|
| `.specify/memory/constitution.md` | Amendment v2.1.0 — Principle IV |
| `index.html` | Nav logo img, nav 4th link, hero CTA restructure |
| `index.css` | `.nav-logo`, `.hero-cta`, `.hero-cta-link`, `.hero-cta-divider`, `body::after` (watermark), mobile suppression |

**No new files** beyond existing assets (already in `assets/`).

---

## JS Line Count Baseline
Current `<script>` block: **13 non-empty source lines** (the IIFE handling speed dial). No JS changes required by this feature. Hard limit is 30 lines.

---

## Open Visual Review Points (for Phase 7)
1. Nav logo height: `36px` specified — confirm visually against the existing nav height (~60px estimated)
2. Watermark size: `auto 55vh` — tune based on actual viewport rendering
3. Watermark opacity: `0.45` — tune in ±0.05 increments if too prominent or too subtle

---

## Clarity Score at Phase 4 Exit: 95%
