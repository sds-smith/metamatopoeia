# Tasks: 002 Layout Refinement — Metamatopoeia Visual Fidelity

**Input**: `specs/002-visual-fidelity-refinement/`  
**Files edited**: `index.css` (646 lines), `index.html` (140 lines)  
**Tests**: Manual visual inspection per `quickstart.md` SC- criteria (not TDD).

## Format: `[ID] [P?] [Story] Description`

---

## Phase 1: Setup

- [x] T001 Confirm branch `002-layout-refinement`; verify `index.css` line 20 reads `--lg-color-text-primary: var(--color-slate)` and `index.html` line 36 reads `class="project-grid"` — baseline confirmed

---

## Phase 2: Foundational — Visual Token Foundation (US4, P2 implemented first)

**⚠️ IMPLEMENTATION ORDER**: Execute these token tasks BEFORE Phase 3 (US1). Despite US4 being P2 in `spec.md`, updating tokens first ensures correct light-on-dark visual output during verification of US1–US3 checkpoints. An implementor following spec priority order (P1 first) would see dark-on-dark text until Phase 6 — acceptable but makes visual debugging harder.

- [x] T002 [US4] `index.css` — **(a)** line 20: change `--lg-color-text-primary: var(--color-slate)` to `--lg-color-text-primary: var(--color-frost)`; **(b)** `.button-primary:hover` rule (approx. line 437): change `background: var(--lg-color-text-primary)` to `background: var(--color-slate)` — **required** to prevent frost-on-frost invisible text in the Hero CTA hover state caused by this token reassignment
- [x] T003 [US4] `index.css` lines 37–41 — replace 2-stop frost-rgb `--lg-glass-reflection` with 4-stop slate-rgb: `linear-gradient(135deg, rgb(var(--color-slate-rgb) / 0.4) 0%, rgb(var(--color-slate-rgb) / 0) 40%, rgb(var(--color-slate-rgb) / 0) 60%, rgb(var(--color-slate-rgb) / 0.15) 100%)`

**Checkpoint (US4)**: Confirm `index.css` line 552 dark-mode block still reads `--lg-color-text-primary: var(--color-frost)` — no edit needed, verify unchanged.

---

## Phase 3: User Story 1 — Background Right-Justified (P1) 🎯 MVP

**Goal**: `body::before` pseudo-element renders background image right-justified at `auto 100vh`.  
**Independent Test**: SC-001 — open at ≥1024px; image on right only. Resize to ≤768px; image covers viewport.

- [x] T004 [US1] `index.css` lines 102–110 — remove `background-image`, `background-size`, `background-position`, `background-attachment` from `body` rule; retain only `min-height: 100vh`
- [x] T005 [US1] `index.css` after line 111 — add `body::before` rule: `content: ""; position: fixed; top: 0; left: 0; right: 0; bottom: 0; z-index: -1; pointer-events: none; background: linear-gradient(var(--lg-page-background-overlay), var(--lg-page-background-overlay)), url("./assets/background-image-profile.jpeg"), linear-gradient(135deg, var(--color-slate) 0%, var(--color-teal) 100%); background-size: auto 100vh; background-position: right; background-repeat: no-repeat;`
- [x] T006 [US1] `index.css` inside `@media (max-width: 768px)` block — add `body::before { background-size: cover; }`

**Checkpoint (US1)**: SC-001 ✅

---

## Phase 4: User Story 2 — Header Transparent (P1)

**Goal**: Header has no glass surface — transparent against page backdrop.  
**Independent Test**: SC-002 — scroll page; no header background/blur/border visible; nav links clickable.

- [x] T007 [US2] `index.css` `.header` rule (lines 245–254) — remove `background`, `backdrop-filter`, `-webkit-backdrop-filter`, `border-bottom`; add `pointer-events: none`
- [x] T008 [US2] `index.css` after `.header` rule — add `.header * { pointer-events: auto; }`

**Checkpoint (US2)**: SC-002 ✅

---

## Phase 5: User Story 3 — Workshop Cards Match Portfolio Design (P1)

**Goal**: Cards use `.media`/`.content`/`.card-actions` structure; images bleed to edges; buttons are glass-surface `<a class="button">`.  
**Independent Test**: SC-003, SC-004 — images flush to card edges; content has 16px padding; no `.project-*` classes in DOM.

### CSS Tasks (`index.css`)

- [x] T009 [US3] `index.css` `.card` rule (lines 183–196) — **(a)** add `position: relative` to the `.card` rule; **(b)** delete the entire `.card::before` rule block (lines 198–207); **(c)** add a new `.card::after` rule: `content: ""; position: absolute; inset: 0; border-radius: inherit; background: var(--lg-glass-reflection); pointer-events: none;`
- [x] T010 [US3] `index.css` `.card-elevated` rule (lines 209–223) — change `border-radius: 20px` to `24px`; remove `padding: 2rem`; add `overflow: hidden` — ensures image-bleed behavior regardless of whether `.card` is also applied to the element
- [x] T011 [US3] `index.css` — delete the entire `.card-elevated::before` rule block (lines 225–234); add a new `.card-elevated::after` rule: `content: ""; position: absolute; inset: 0; border-radius: inherit; background: var(--lg-glass-reflection); pointer-events: none;`
- [x] T012 [US3] `index.css` `.hero-card` rule (lines 298–301) — add `padding: 2rem`
- [x] T013 [US3] `index.css` `.contact-card` rule (lines 363–366) — add `padding: 2rem`
- [x] T014 [US3] `index.css` after `.card-elevated:hover` rule — add three new rules: `.media { position: relative; z-index: 1; display: block; width: 100%; object-fit: cover; background-size: cover; background-repeat: no-repeat; background-position: center; }` and `.content { position: relative; z-index: 1; padding: 16px; }` and `.card-actions { display: flex; gap: 8px; margin-top: 16px; flex-wrap: wrap; }`
- [x] T015 [US3] `index.css` after `.project-grid` section — add two new rules: `.portfolio-links { display: flex; flex-direction: column; gap: 24px; }` and `.layout-list .card { width: 100%; }`
- [x] T016 [US3] `index.css` `.button` rule (lines 417–429) — replace `padding: 0.75rem 1.5rem; border: none; font-weight: 600; font-size: var(--lg-typography-size-sm)` with `padding: 8px 16px; border: 1px solid var(--lg-glass-border); background: var(--lg-glass-surface); color: var(--lg-color-text-muted); font-size: var(--lg-typography-size-xs); text-decoration: none;`
- [x] T017 [US3] `index.css` — delete rules for `.project-grid` (lines 313–317), `.project-card` (319–323), `.project-image` (325–331), `.project-content` (333–337), `.project-description` (346–351), `.project-actions` (353–357); also delete `.project-grid` override inside `@media (max-width: 768px)` (lines 619–621)

### HTML Tasks (`index.html`) — Parallel with CSS tasks above (different file)

- [x] T018 [P] [US3] `index.html` line 36 — change `class="project-grid"` to `class="portfolio-links"`
- [x] T019 [P] [US3] `index.html` Cup card (lines 37–48) — `article`: remove `project-card`; `img`: `project-image` → `media`; `div`: `project-content` → `content`; `p`: `project-description` → `portfolio-description`; `div`: `project-actions` → `card-actions`; all `<a>` elements: remove `button-secondary` class
- [x] T020 [P] [US3] `index.html` Liquid Glass UI card (lines 49–58) — same remapping as T019
- [x] T021 [P] [US3] `index.html` Discover Breweries card (lines 59–68) — same remapping as T019
- [x] T022 [P] [US3] `index.html` Assemble the Jams card (lines 69–79) — same remapping as T019

**Checkpoint (US3)**: SC-003, SC-004 ✅ — images flush to edges; 16px content padding; no `.project-*` in DOM; buttons are `<a class="button">`

---

## Phase 6: User Story 5 — FAB Paper Plane Icon (P2)

**Goal**: FAB shows paper plane icon, static across open/closed states.  
**Independent Test**: SC-007 — FAB icon is paper plane; click to open; icon unchanged.

- [x] T023 [US5] `index.css` lines 493–495 — delete rule `.speed-dial-checkbox:checked ~ .speed-dial-fab .speed-dial-icon { transform: rotate(45deg); }`
- [x] T024 [P] [US5] `index.html` line 113 — replace FAB `<svg>` contents from two `<line>` ("+") to `<line x1="22" y1="2" x2="11" y2="13"></line><polygon points="22 2 15 22 11 13 2 9 22 2"></polygon>`; add `stroke-linecap="round" stroke-linejoin="round"` to `<svg>` attributes

**Checkpoint (US5)**: SC-007 ✅ — paper plane icon; no rotation on open

---

## Phase 7: Polish & Validation

- [x] T025 Run all 11 SC- verification steps from `quickstart.md` against the modified `index.html` and `index.css`; confirm SC-008 (no console errors), SC-009 (zero network requests), SC-010 (JS ≤30 lines), SC-011 (no overflow at 280px)

---

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1 (Setup)**: No dependencies — start immediately
- **Phase 2 (Foundational)**: After Phase 1 — sets visual baseline for all verification
- **Phases 3–6 (User Stories)**: Each can start after Phase 2; logically independent of each other
- **Phase 7 (Polish)**: After all user story phases

### User Story Dependencies

- **US1, US2, US3, US5**: Independent — no cross-story CSS/HTML dependencies
- **US4** (tokens): Done in Phase 2; no separate phase needed

### Parallel Opportunities

- T018–T022 (HTML card edits) can be done concurrently with T009–T017 (CSS edits) — different files
- T024 (HTML FAB SVG) can be done concurrently with T023 (CSS FAB rule removal) — different files
- T019–T022 are logically independent (different cards) but same file — edit sequentially

---

## Parallel Example: User Story 3

```text
Parallel track A (index.css):  T009 → T010 → T011 → T012 → T013 → T014 → T015 → T016 → T017
Parallel track B (index.html): T018 → T019 → T020 → T021 → T022
Both tracks run simultaneously; merge when both complete before Checkpoint US3.
```

---

## Implementation Strategy

### MVP First (US1 — Background, single SC- criterion)

1. Complete T001 (Setup)
2. Complete T002–T003 (Tokens)
3. Complete T004–T006 (Background)
4. **STOP and VALIDATE** SC-001: right-justified image visible at desktop viewport

### Incremental Delivery

1. T001 → T002–T003 → Foundation
2. T004–T006 → US1 ✅ SC-001
3. T007–T008 → US2 ✅ SC-002
4. T009–T022 → US3 ✅ SC-003, SC-004
5. T023–T024 → US5 ✅ SC-007
6. T025 → Full validation ✅ SC-008–SC-011

---

## Notes

- All 5 changes are in exactly 2 files: `index.css` and `index.html`
- `<script>` block in `index.html` is NOT touched — JS unchanged
- `assets/` directory is NOT touched
- Section structure (`<section id="...">`, `<h2>`, nav) is NOT touched
- `.button-secondary` CSS rule retained in stylesheet but no longer applied to any HTML element
- Line numbers are approximate baselines — verify before editing if tasks are done out of order
