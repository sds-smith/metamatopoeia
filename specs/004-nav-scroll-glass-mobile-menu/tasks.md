# Tasks: 004 Nav Scroll-Glass & Mobile Hamburger Menu

**Input**: Design documents from `specs/004-nav-scroll-glass-mobile-menu/`  
**Prerequisites**: plan.md ✅ spec.md ✅ research.md ✅ data-model.md ✅ quickstart.md ✅

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel when edits target different files or independent CSS blocks
- **[Story]**: User story this task belongs to (US1 = Scroll Glass, US2 = Hamburger Menu, US3 = Logo Mobile-Only)

---

## Phase 1: Setup (Tokens & HTML Foundation)

**Purpose**: Add shared tokens and the accessible checkbox foundation needed by the CSS-only mobile menu.

- [ ] T001 Add `--lg-nav-scroll-range: 80px` and `--lg-nav-overlay-bg-opacity: 0.88` to the `:root` token block in `index.css` after `--lg-glass-transition-easing`
- [ ] T002 [P] Add `<input type="checkbox" id="nav-menu-toggle" class="nav-menu-checkbox" aria-label="Toggle navigation menu" />` as the first child of `<body>` in `index.html` before `<header class="header">`

**Checkpoint**: Two tokens exist in `:root`; checkbox is first child of `<body>`, remains keyboard-focusable, and does not use `aria-hidden`.

---

## Phase 2: User Story 1 — Scroll-Aware Nav Surface (Priority: P1) 🎯 MVP

**Goal**: At scroll = 0 the nav is fully transparent; by scroll = 80px it transitions to a liquid glass surface; returns to transparent on scroll-back.

**Independent Test**: Open `index.html` in Chrome 115+. At page load (scroll = 0) the nav has no background or blur. Scroll 80px+ — `.header` shows frosted background, bottom border, and shadow. Scroll back to top — returns to transparent. Under `prefers-reduced-motion`, nav shows a static glass surface without animation.

### Implementation for User Story 1

- [ ] T003 [US1] Add a `/* NAV SCROLL ANIMATION */` section in `index.css` after the HEADER & NAVIGATION section containing `@keyframes nav-glass-activate` with a transparent `from` state (`background: transparent; backdrop-filter: blur(0px); -webkit-backdrop-filter: blur(0px); box-shadow: none; border-bottom: 1px solid transparent;`) and a glass `to` state (`background: var(--lg-glass-surface); backdrop-filter: blur(var(--lg-glass-blur)); -webkit-backdrop-filter: blur(var(--lg-glass-blur)); box-shadow: var(--lg-glass-shadow-md); border-bottom: 1px solid var(--lg-glass-border);`)
- [ ] T004 [US1] Add `animation-name: nav-glass-activate; animation-timing-function: linear; animation-fill-mode: both; animation-timeline: scroll(root); animation-range: 0px var(--lg-nav-scroll-range);` to the existing `.header` rule in `index.css`
- [ ] T005 [US1] In the `@media (prefers-reduced-motion: reduce)` block in `index.css`, add a `.header` override that sets `animation: none; background: var(--lg-glass-surface); backdrop-filter: blur(var(--lg-glass-blur)); -webkit-backdrop-filter: blur(var(--lg-glass-blur)); box-shadow: var(--lg-glass-shadow-md); border-bottom: 1px solid var(--lg-glass-border);`

**Checkpoint**: US1 fully functional. Scroll animation works. Reduced-motion fallback applies static glass.

---

## Phase 3: User Story 2 — CSS-Only Mobile Hamburger Menu (Priority: P2)

**Goal**: On mobile-first base styles, nav links are hidden behind a full-screen slide-in overlay. A visible hamburger label toggles a keyboard-focusable hidden checkbox. The icon morphs to ✕ when open. Zero JavaScript.

**Independent Test**: Resize browser to 375px width. Nav bar shows brand + hamburger icon; nav links are hidden. Tab to the hamburger control (focus is on the checkbox but visibly shown on the hamburger), press Space → full-screen glass overlay slides in from top with 4 centered nav links; hamburger morphs to ✕. Press Space again or click ✕ → overlay closes. Closed overlay links are not reachable by Tab.

### Implementation for User Story 2

- [ ] T006 [US2] In `index.html`, inside `<nav class="nav">`, add `<label for="nav-menu-toggle" class="nav-hamburger">` with three `<span class="hamburger-line" aria-hidden="true"></span>` children, positioned immediately after `.nav-brand` and before `<ul class="nav-links">`
- [ ] T007 [P] [US2] In the HEADER & NAVIGATION section of `index.css`, add `.nav-menu-checkbox` using an accessible visually-hidden pattern: `position: absolute; width: 1px; height: 1px; margin: -1px; padding: 0; border: 0; overflow: hidden; clip: rect(0 0 0 0); clip-path: inset(50%); white-space: nowrap;`
- [ ] T008 [P] [US2] In the HEADER & NAVIGATION section of `index.css`, update `.nav-brand` to include `display: flex; align-items: center; gap: 0.5rem; position: relative; z-index: 1;` while preserving its existing typography and color declarations
- [ ] T009 [P] [US2] In the HEADER & NAVIGATION section of `index.css`, add mobile-first `.nav-hamburger { display: flex; flex-direction: column; gap: 5px; width: 24px; cursor: pointer; padding: 4px; position: relative; z-index: 1; }`
- [ ] T010 [P] [US2] In the HEADER & NAVIGATION section of `index.css`, add `.hamburger-line { width: 100%; height: 2px; background: var(--lg-color-text-primary); border-radius: 2px; transition: transform var(--lg-glass-transition-duration) var(--lg-glass-transition-easing), opacity var(--lg-glass-transition-duration) var(--lg-glass-transition-easing); }`
- [ ] T011 [US2] In the HEADER & NAVIGATION section of `index.css`, replace the existing `.nav-links` base rule with mobile-first overlay styles: `display: flex; position: fixed; inset: 0; background: rgb(var(--color-slate-rgb) / var(--lg-nav-overlay-bg-opacity)); backdrop-filter: blur(var(--lg-glass-blur)); -webkit-backdrop-filter: blur(var(--lg-glass-blur)); flex-direction: column; align-items: center; justify-content: center; gap: 2rem; list-style: none; transform: translateY(-100%); visibility: hidden; opacity: 0; z-index: 0; transition: transform var(--lg-glass-transition-duration) var(--lg-glass-transition-easing), opacity var(--lg-glass-transition-duration) var(--lg-glass-transition-easing), visibility 0ms var(--lg-glass-transition-duration);`
- [ ] T012 [US2] In the HEADER & NAVIGATION section of `index.css`, add `.nav-links .nav-link { font-size: var(--lg-typography-size-lg); min-height: 48px; display: flex; align-items: center; padding: 0 1.5rem; color: var(--lg-color-text-primary); }`
- [ ] T013 [US2] In the HEADER & NAVIGATION section of `index.css`, add `.nav-menu-checkbox:focus-visible ~ .header .nav-hamburger { outline: 2px solid var(--lg-color-accent-primary); outline-offset: 4px; border-radius: 4px; }`
- [ ] T014 [US2] In the HEADER & NAVIGATION section of `index.css`, add `.nav-menu-checkbox:checked ~ .header .nav-links { transform: translateY(0); visibility: visible; opacity: 1; transition: transform var(--lg-glass-transition-duration) var(--lg-glass-transition-easing), opacity var(--lg-glass-transition-duration) var(--lg-glass-transition-easing), visibility 0ms; }`
- [ ] T015 [US2] In the HEADER & NAVIGATION section of `index.css`, add hamburger-to-X selectors: `.nav-menu-checkbox:checked ~ .header .hamburger-line:nth-child(1) { transform: translateY(7px) rotate(45deg); }`, `.nav-menu-checkbox:checked ~ .header .hamburger-line:nth-child(2) { opacity: 0; transform: scaleX(0); }`, and `.nav-menu-checkbox:checked ~ .header .hamburger-line:nth-child(3) { transform: translateY(-7px) rotate(-45deg); }`
- [ ] T016 [US2] Remove the old desktop-first mobile overrides for `.nav` and `.nav-links` from the existing `@media (max-width: 768px)` block in `index.css` so they do not conflict with mobile-first base nav behavior
- [ ] T017 [US2] Add a desktop override block `@media (min-width: 769px)` in `index.css` that sets `.nav-hamburger { display: none; }` and restores `.nav-links` to desktop row layout: `position: static; inset: auto; flex-direction: row; justify-content: flex-end; align-items: center; gap: 2rem; transform: none; visibility: visible; opacity: 1; z-index: auto; background: transparent; backdrop-filter: none; -webkit-backdrop-filter: none; transition: none;`
- [ ] T018 [US2] In the `@media (prefers-reduced-motion: reduce)` block in `index.css`, add `.nav-links { transition: none; }` and `.hamburger-line { transition: none; }`

**Checkpoint**: US2 fully functional. Hamburger visible on mobile, overlay opens/closes, icon morphs correctly, checkbox is keyboard-operable, links are unavailable to Tab when closed, and desktop nav links remain visible.

---

## Phase 4: User Story 3 — Nav-Brand Logo Mobile-Only (Priority: P3)

**Goal**: `<img class="nav-logo">` is visible in mobile-first base styles and hidden on desktop (> 768px).

**Independent Test**: At mobile viewport (≤ 768px), DevTools computed styles on `img.nav-logo` show `display: block`. At desktop viewport (> 768px), computed `display` is `none`.

### Implementation for User Story 3

- [ ] T019 [P] [US3] Preserve `.nav-logo { display: block; height: 36px; width: auto; }` as the mobile-first base rule in the HEADER & NAVIGATION section of `index.css`
- [ ] T020 [P] [US3] In the desktop `@media (min-width: 769px)` block in `index.css`, add `.nav-logo { display: none; }`

**Checkpoint**: US3 fully functional. Logo visible on mobile, hidden on desktop, and `.nav-brand` text remains visible everywhere.

---

## Phase 5: Polish & Cross-Cutting Concerns

**Purpose**: Verification and constitution compliance confirmation.

- [ ] T021 Run all quickstart.md acceptance criteria (AC-1, AC-2, AC-3) and negative cases (N-1 through N-5) in `index.html` opened directly in browser
- [ ] T022 [P] Verify JS non-empty line count in `index.html` `<script>` block is still exactly 26 (≤ 30 budget; zero lines consumed by this feature)
- [ ] T023 [P] Verify `.nav-brand` text "Metamatopoeia" is visible on both desktop and mobile viewports
- [ ] T024 [P] Verify `index.css` contains no new hex, hsl, rgb, or named color values outside the approved palette-derived CSS custom properties

---

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1 (Setup)**: No dependencies — start immediately
- **Phase 2 (US1)**: Depends on T001
- **Phase 3 (US2)**: Depends on T001 + T002 + T006
- **Phase 4 (US3)**: Depends on desktop override block existence from T017 if implemented separately
- **Phase 5 (Polish)**: Depends on all US phases complete

### User Story Dependencies

- **US1 (P1)**: Unblocked after T001. Pure CSS scroll behavior.
- **US2 (P2)**: Unblocked after T001, T002, and T006. HTML + CSS behavior.
- **US3 (P3)**: Can be implemented with T017 or after T017. Pure CSS visibility behavior.

### Parallel Opportunities

- T001 and T002 can run together because they edit different files
- T007, T008, T009, and T010 are independent CSS rule additions
- T019 and T020 can run alongside US2 once desktop override placement is known
- T022, T023, and T024 can run independently during final verification

---

## Parallel Example: US2 CSS Foundations

```text
T007: Add accessible visually-hidden .nav-menu-checkbox rule in index.css
T008: Add stacking/flex additions to .nav-brand in index.css
T009: Add mobile-first .nav-hamburger rule in index.css
T010: Add .hamburger-line rule in index.css
```

---

## Implementation Strategy

### MVP First (US1 Only — Scroll Glass)

1. Complete T001
2. Complete T003, T004, T005
3. Stop and validate scroll animation and reduced-motion fallback

### Incremental Delivery

1. Phase 1 → tokens + accessible checkbox
2. Phase 2 → scroll glass ✅ testable
3. Phase 3 → keyboard-operable hamburger menu ✅ testable
4. Phase 4 → logo mobile-only ✅ testable
5. Phase 5 → polish + full verification

---

## Notes

- No tests requested in spec — test tasks omitted per workflow rules
- No contracts directory — N/A for client-side visual feature
- The `<script>` block at bottom of `index.html` must not be modified
- All CSS edits land in `index.css` only; all HTML edits land in `index.html` only
- Do not use `aria-hidden="true"` on the checkbox
- Do not add `tabindex` to `.nav-hamburger`; keyboard operation belongs to the checkbox
- Do not append new mobile nav rules alongside the old `@media (max-width: 768px)` `.nav` / `.nav-links` rules; remove or replace the old conflicting rules
