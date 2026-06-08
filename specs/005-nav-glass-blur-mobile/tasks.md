# Tasks: Bug 005 — Nav Glass Scroll Transition Mobile Blur

**Input**: Design documents from `specs/005-nav-glass-blur-mobile/`
**Prerequisites**: plan.md ✓, spec.md ✓, research.md ✓

**Format**: `[ID] [P?] [Story?] Description`

- **[P]**: Can run in parallel (different sections, no dependencies)
- **[Story]**: User story label — all tasks here are [US1] (single bug fix)

---

## Phase 1: Pre-implementation Verification

**Purpose**: Confirm live CSS matches the spec assumptions before making changes.

- [x] T001 Read `index.css` lines 299–310 — VERIFIED: all 5 locations match spec.md §4 current state (`.header` rule), 426–441 (`@keyframes nav-glass-activate`), 795–802 (`prefers-reduced-motion` `.header` block), and 819–826 (`prefers-reduced-transparency` selector list); confirm they match the "current state" described in `specs/005-nav-glass-blur-mobile/spec.md` §4 before proceeding

**Checkpoint**: Current state verified — five targeted edit locations confirmed

---

## Phase 2: Bug Fix Implementation (Priority: P1) 🎯 MVP

**User Story (US1)**: As a mobile visitor on iOS Chrome, I want the nav glass blur to apply reliably on scroll so the navigation header is clearly differentiated from the page content beneath it.

**Goal**: Replace the animated-`backdrop-filter` approach (WebKit compositor race root cause) with a static-`backdrop-filter` `::before` pseudo-element whose `opacity` is animated via the scroll timeline instead.

**Independent Test**: Open `index.html` locally; scroll down 100px; confirm nav header shows glass surface with backdrop blur. Scroll back to top; confirm nav is fully transparent. Test in desktop Chrome DevTools mobile emulation.

### Implementation for User Story 1

- [x] T002 [US1] In `index.css`, remove the 5 scroll animation properties from the `.header` rule (lines 305–309) — DONE: .header now has no animation properties: delete `animation-name: nav-glass-activate;`, `animation-timing-function: linear;`, `animation-fill-mode: both;`, `animation-timeline: scroll(root);`, and `animation-range: 0px var(--lg-nav-scroll-range);` — leaving `.header` with only `position: sticky; top: 0; z-index: 1000; padding: 1rem 2rem; pointer-events: none;`

- [x] T003 [US1] In `index.css`, rename `@keyframes nav-glass-activate` to `@keyframes nav-glass-surface-reveal` — DONE: keyframe now has opacity: 0 → 1 only and replace the entire multi-property keyframe body (lines 426–441) with a single opacity transition: `from { opacity: 0; }` and `to { opacity: 1; }`

- [x] T004 [US1] In `index.css`, add a `.header::before` rule immediately after the `@keyframes nav-glass-surface-reveal` block — DONE: static backdrop-filter + opacity animation applied. The rule must set `content: ''; position: absolute; inset: 0; z-index: -1;` then the static glass properties `backdrop-filter: blur(var(--lg-glass-blur)); -webkit-backdrop-filter: blur(var(--lg-glass-blur)); background: var(--lg-glass-surface); box-shadow: var(--lg-glass-shadow-md); border-bottom: 1px solid var(--lg-glass-border);` then the scroll-driven animation `animation-name: nav-glass-surface-reveal; animation-timing-function: linear; animation-fill-mode: both; animation-timeline: scroll(root); animation-range: 0px var(--lg-nav-scroll-range);` — see spec.md §4.3 for exact property set

- [x] T005 [US1] In `index.css`, inside the `@media (prefers-reduced-motion: reduce)` block — DONE: replaced .header override with .header::before override: remove the entire existing `.header { animation: none; background: ...; backdrop-filter: ...; -webkit-backdrop-filter: ...; box-shadow: ...; border-bottom: ...; }` rule (lines 795–802) and add `.header::before { animation: none; opacity: 1; }` in its place — see spec.md §4.4

- [x] T006 [US1] In `index.css`, inside the `@media (prefers-reduced-transparency: reduce)` block — DONE: replaced .header with .header::before in selector list: in the multi-selector rule at lines 819–825, replace `.header` with `.header::before` in the selector list so that `backdrop-filter: none; -webkit-backdrop-filter: none;` applies to `.header::before` — see spec.md §4.5

**Checkpoint**: US1 fully implemented. All 5 edit locations modified. Scroll animation root cause eliminated.

---

## Phase 3: Polish & Verification

**Purpose**: Cross-browser visual confirmation and reduced-motion/transparency regression check.

- [ ] T007 ⏳ MANUAL: Open `index.html` directly from the filesystem in a desktop browser; scroll to 80px+ and confirm glass surface (tint + blur) activates; scroll to top — confirm transparent; test nav link click-through while glass active; scroll to 80px and beyond — confirm nav glass surface (background tint + blur) activates; scroll back to top — confirm nav returns to fully transparent; confirm no visual regression in hamburger menu, hero, or other sections

- [ ] T008 ⏳ MANUAL: In DevTools, set `prefers-reduced-motion: reduce` — confirm persistent static glass; set `prefers-reduced-transparency: reduce` — confirm `backdrop-filter: none` with high-opacity fallback; confirm script block still at 26 non-empty lines — confirm nav shows a persistent static glass surface from page load (no animation); set `prefers-reduced-transparency: reduce` — confirm `backdrop-filter` is removed and nav background uses the high-opacity solid fallback from `--lg-glass-bg-opacity: 0.98`

---

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1 (Verify)**: No dependencies — start immediately
- **Phase 2 (Fix)**: Depends on Phase 1 verification complete
  - T002, T003, T004 are sequential: T002 decouples the old keyframe; T003 renames it; T004 adds `::before` referencing the new keyframe name
  - T005 and T006 are logically independent of T002–T004 (different media query blocks) but are in the same file — apply in sequence during the same editing session
- **Phase 3 (Polish)**: Depends on all Phase 2 tasks complete

### Within Phase 2

```
T002 → T003 → T004   (sequential: same CSS section, keyframe rename dependency)
T005                  (independent: different media query section)
T006                  (independent: different media query section)
```

Apply T005 and T006 after T004 to complete all 5 changes before any verification.

### Parallel Opportunities

None — all edits target `index.css`. Apply sequentially T002 → T003 → T004 → T005 → T006.

---

## Implementation Strategy

### MVP (This entire fix IS the MVP)

1. Complete Phase 1: Verify current state
2. Complete Phase 2: Apply T002 → T003 → T004 → T005 → T006
3. **STOP and VALIDATE**: Phase 3 visual verification
4. If verified: proceed to `/speckit.git.commit`

### Incremental Safety

Each task is a self-contained CSS block edit. If a task introduces a regression, it can be reverted independently. The order T002 → T003 → T004 is the safe path:

- After T002: `.header` has no animation → nav is permanently transparent (expected intermediate state)
- After T003: `@keyframes nav-glass-surface-reveal` exists but isn't used yet → no change
- After T004: `::before` with opacity animation activates → fix is live

---

## Notes

- No [P] markers used — all changes are in `index.css` (same file, sequential required)
- No tests generated — spec.md does not request TDD; manual visual verification is sufficient for a CSS property change
- All task descriptions reference spec.md section numbers for authoritative property values
- The `@keyframes nav-glass-activate` name is fully replaced — no other rules reference it after T002
