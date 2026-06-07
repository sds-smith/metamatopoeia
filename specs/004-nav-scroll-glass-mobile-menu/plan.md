# Implementation Plan: 004 Nav Scroll-Glass & Mobile Hamburger Menu

**Branch**: `004-nav-scroll-glass-mobile-menu` | **Date**: 2026-06-07 | **Spec**: `specs/004-nav-scroll-glass-mobile-menu/spec.md`  
**Input**: Feature specification from `specs/004-nav-scroll-glass-mobile-menu/spec.md`

## Summary

Three targeted enhancements to the top navigation in `index.html` and `index.css`: (1) scroll-driven transparent-to-glass surface transition via CSS Scroll-Driven Animations (`animation-timeline: scroll(root)`); (2) CSS-only mobile hamburger menu using a visually hidden but keyboard-focusable checkbox and a full-screen slide-in overlay; (3) `<img class="nav-logo">` visible in mobile-first base styles and hidden on desktop only. Zero new JavaScript lines consumed. Two new CSS custom property tokens added.

## Technical Context

**Language/Version**: HTML5, CSS3 (vanilla — zero preprocessors, zero transpilation)  
**Primary Dependencies**: None — zero-dependency by constitution (Principle I)  
**Storage**: N/A — static files; `./assets/` directory unchanged  
**Testing**: Manual visual inspection + browser DevTools (scroll behavior, mobile viewport simulation, hamburger toggle, logo visibility)  
**Target Platform**: Chrome 115+, Firefox 110+, Safari 17.4+ for full feature; older browsers degrade gracefully (transparent nav, no broken layout)  
**Project Type**: Static brand website — single HTML page  
**Performance Goals**: Zero network requests on cold load; instant paint from filesystem  
**Constraints**: ≤30 JS non-empty source lines (26 existing, **0 new lines consumed**); single `index.html` + `index.css`; 4-color palette only; `--lg-{group}-{subgroup}-{token}` naming; no build tools  
**Scale/Scope**: 2 source files — ~6 HTML line additions, ~110 CSS line additions; no new files; no new assets

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

| Principle                      | Status  | Evidence                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------ | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| I. Zero-Dependency, Build-Free | ✅ PASS | Zero new JS lines; CSS Scroll-Driven Animations only; no CDN; no `<script src>`; hamburger is pure CSS checkbox pattern                                                                                                                                                                                                                                |
| II. Liquid Glass Design System | ✅ PASS | Two new tokens (`--lg-nav-scroll-range`, `--lg-nav-overlay-bg-opacity`) follow `--lg-{group}-{subgroup}-{token}`; all transitions and glass effects via CSS `@keyframes` and custom properties                                                                                                                                                         |
| III. Color Palette             | ✅ PASS | Overlay background uses `rgb(var(--color-slate-rgb) / var(--lg-nav-overlay-bg-opacity))`; glass surface reuses `var(--lg-glass-surface)`; no new hex values                                                                                                                                                                                            |
| IV. Single-Page Architecture   | ✅ PASS | Nav link hrefs and text preserved (home/workshop/meet the founder/contact); three required in-page anchors unchanged; no new pages or routing                                                                                                                                                                                                          |
| V. Accessibility & Performance | ✅ PASS | Checkbox remains keyboard-focusable with `aria-label`; `.nav-hamburger` receives visible focus styling through `:focus-visible`; `visibility: hidden` removes closed overlay links from tab order; touch targets ≥ 48px; decorative hamburger spans use `aria-hidden="true"`; responsive nav styles are mobile-first with desktop `min-width` override |
| Brand & Content                | ✅ PASS | No copy changes; contact channels unchanged; Metamatopoeia branding unchanged                                                                                                                                                                                                                                                                          |

**Gate result: PASS — cleared to proceed to Phase 0.**

**Post-Phase 1 re-check**: ✅ PASS — `data-model.md` token inventory confirms zero palette violations; no new external resources; JS line count unchanged at 26.

## Project Structure

### Documentation (this feature)

```text
specs/004-nav-scroll-glass-mobile-menu/
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
├── index.html    # EDIT — add accessible nav-menu-checkbox before <header>; add nav-hamburger <label> inside .nav
├── index.css     # EDIT — 2 new tokens; @keyframes; .header scroll animation; mobile-first hamburger/overlay rules; desktop overrides
└── assets/       # NO CHANGE
```

**Structure Decision**: Flat single-project layout per constitution Principle I (zero-dependency, build-free). No subdirectories in source. Two files edited; no files created or deleted. Verification is manual browser inspection against acceptance criteria in `quickstart.md`.

## Complexity Tracking

No violations detected. All three change groups are within constitutional bounds. No Complexity Tracking entries required.
