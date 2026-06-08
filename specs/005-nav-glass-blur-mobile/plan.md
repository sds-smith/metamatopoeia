# Implementation Plan: Bug 005 — Nav Glass Scroll Transition Mobile Blur

**Branch**: `005-nav-glass-blur-mobile` | **Date**: 2026-06-08 | **Spec**: [spec.md](./spec.md)
**Input**: Feature specification from `specs/005-nav-glass-blur-mobile/spec.md`

## Summary

On iOS Chrome (WebKit), `backdrop-filter` blur on the nav scroll-glass transition is intermittently absent. Root cause: WebKit does not reliably animate `backdrop-filter` inside a CSS Scroll-Driven Animation on a `position: sticky` element. Fix: move all glass surface properties (including static `backdrop-filter`) to a `::before` pseudo-element; animate only `opacity` (0→1) via the scroll timeline. Eliminates the animated-`backdrop-filter` compositor race entirely.

**Edit surface**: `index.css` only — 5 targeted changes across 4 CSS sections. Zero HTML, zero JS.

## Technical Context

**Language/Version**: Vanilla CSS (no version) + HTML5  
**Primary Dependencies**: N/A — zero-dependency, build-free static site  
**Storage**: N/A  
**Testing**: Manual visual verification on iOS Chrome (WebKit); cross-check on desktop Chrome/Firefox/Safari DevTools  
**Target Platform**: Static file, filesystem-openable; browsers: Chrome 115+, Firefox 110+, Safari 17.4+  
**Project Type**: Single-page static website (`index.html` + `index.css` + inline `<script>`)  
**Performance Goals**: No new performance impact; `backdrop-filter` is already GPU-composited; `opacity` animation adds no overhead  
**Constraints**: ≤30 non-empty JS lines (26 used; fix adds 0); single `index.css` stylesheet; no build tools  
**Scale/Scope**: 5 targeted CSS edits in 1 file; no new files

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

| Principle                      | Status  | Notes                                                                                                             |
| ------------------------------ | ------- | ----------------------------------------------------------------------------------------------------------------- |
| I. Zero-Dependency, Build-Free | ✅ PASS | No new dependencies; no JS changes; no build step                                                                 |
| II. Liquid Glass Design System | ✅ PASS | Glass effects remain CSS-only; `backdrop-filter` moves to `::before` but stays in CSS; no new tokens              |
| III. Color Palette             | ✅ PASS | No new color values; all existing `--lg-*` tokens reused                                                          |
| IV. Single-Page Architecture   | ✅ PASS | No HTML changes; no routing; no JS page transitions                                                               |
| V. Accessibility & Performance | ✅ PASS | `::before` is decorative, semantically invisible; `pointer-events: none` inherited; no keyboard navigation impact |

**Post-Phase-1 re-check**: All principles still pass. No complexity violations.

## Project Structure

### Documentation (this feature)

```text
specs/005-nav-glass-blur-mobile/
├── spec.md        ✓ created
├── plan.md        ✓ this file
├── research.md    ✓ Phase 0 output
└── tasks.md         Phase 2 output (/speckit.tasks — not created by /speckit.plan)
```

_No `data-model.md`, `contracts/`, or `quickstart.md` — pure CSS bug fix, no data entities or external interfaces._

### Source Code (repository root)

```text
index.html          unchanged
index.css           ← single file edited (5 changes, 4 sections)
```

**Structure Decision**: Single-file static site. All changes confined to `index.css`. No new files created in the source tree.

## Complexity Tracking

> No constitution violations. Table left empty intentionally.

| Violation | Why Needed | Simpler Alternative Rejected Because |
| --------- | ---------- | ------------------------------------ |
| —         | —          | —                                    |
