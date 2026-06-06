# Implementation Plan: 002 Layout Refinement — Metamatopoeia Visual Fidelity

**Branch**: `002-layout-refinement` | **Date**: 2026-06-06 | **Spec**: `specs/002-visual-fidelity-refinement/spec.md`  
**Input**: Feature specification from `specs/002-visual-fidelity-refinement/spec.md`

## Summary

A surgical polish pass on `index.html` and `index.css` to close 5 visual fidelity gaps against the model site `sds-smith/html_portfolio`. Changes are confined to CSS rule modifications and HTML class/element remapping — no new files, no structural changes, no JavaScript changes. All 30 FRs are addressed by targeted edits to exactly 2 files.

## Technical Context

**Language/Version**: HTML5, CSS3 (vanilla — zero preprocessors, zero transpilation)  
**Primary Dependencies**: None — zero-dependency by constitution (Principle I)  
**Storage**: N/A — static files; local `./assets/` directory unchanged  
**Testing**: Manual visual inspection + browser DevTools (computed styles, network tab, box model); 11 measurable SC- criteria  
**Target Platform**: Modern browsers (Chrome, Firefox, Safari, Edge) — static filesystem open, no server required  
**Project Type**: Static brand website — single HTML page  
**Performance Goals**: Zero network requests on cold load; instant paint from filesystem  
**Constraints**: ≤30 JS non-empty source lines; single `index.html` + `index.css`; 4-color palette only; `--lg-{group}-{subgroup}-{token}` naming; no build tools  
**Scale/Scope**: 2 source files (~140 HTML lines, ~650 CSS lines); flat repo root layout; no modules, no build pipeline

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

| Principle                      | Status  | Evidence                                                                                                                                                                                                                    |
| ------------------------------ | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| I. Zero-Dependency             | ✅ PASS | Only vanilla HTML/CSS edits; no new libraries, no CDN references, no `<script src>`                                                                                                                                         |
| II. Liquid Glass Design System | ✅ PASS | New tokens follow `--lg-{group}-{subgroup}-{token}`; all effects via CSS; `--lg-page-background-overlay` token used for overlay                                                                                             |
| III. Color Palette             | ✅ PASS | `body::before` overlay uses `var(--lg-page-background-overlay)` (derives from `--color-slate-rgb`); reflection uses `--color-slate-rgb`; text uses `--color-frost`; fallback gradient uses `--color-slate` + `--color-teal` |
| IV. Single-Page Architecture   | ✅ PASS | No structural changes; 3-section layout, nav anchors, and section IDs untouched                                                                                                                                             |
| V. Accessibility & Performance | ✅ PASS | `<h3>` retained for titles; `.header *` gets `pointer-events: auto`; `<a class="button">` valid HTML; `loading="lazy"` on images retained; mobile breakpoint at 768px preserved                                             |
| Brand & Content                | ✅ PASS | No content copy changes; no personal names; all contact channel URLs unchanged                                                                                                                                              |

**Gate result: PASS — cleared to proceed to Phase 0.**

**Post-Phase 1 re-check**: ✅ PASS — `data-model.md` class inventory confirms zero palette violations; no new external resources introduced.

## Project Structure

### Documentation (this feature)

```text
specs/002-visual-fidelity-refinement/
├── plan.md              # This file
├── spec.md              # Feature specification (copy of .specify/memory/spec.md)
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
└── tasks.md             # Phase 2 output (via /speckit.tasks — NOT created here)
```

### Source Code (repository root)

```text
/workspaces/metamatopoeia/
├── index.html           # EDIT — workshop card HTML restructure + FAB SVG swap
├── index.css            # EDIT — background, header, card, color token, button, FAB CSS
└── assets/              # NO CHANGE — all 4 card media images already present
```

**Structure Decision**: Flat single-project layout per constitution Principle I (zero-dependency, build-free). No subdirectories in source. Verification is manual browser inspection against 11 measurable SC- criteria in `quickstart.md`.

## Complexity Tracking

No violations detected. All 5 change groups are within constitutional bounds. No Complexity Tracking entries required.
