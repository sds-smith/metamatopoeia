# Tasks: Metamatopoeia Website

**Input**: Design documents from `/specs/001-metamatopoeia-website/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Manual browser testing only (per constitution - no automated test framework)

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Static website**: Repository root (`index.html`, `index.css`, `assets/`)
- No build tools, no framework structure
- All files at repository root level

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and asset preparation

- [x] T001 Create assets directory at repository root
- [ ] T002 Copy background image from reference repository to ./assets/background-image-profile.jpeg
- [ ] T003 [P] Copy Cup project card image from reference repository to ./assets/card-media-cup.png
- [ ] T004 [P] Copy Liquid Glass UI project card image from reference repository to ./assets/card-media-liquid-glass-ui.png
- [ ] T005 [P] Copy Discover Breweries project card image from reference repository to ./assets/card-media-discover-breweries.png
- [ ] T006 [P] Copy Assemble the Jams project card image from reference repository to ./assets/card-media-atj.png

**Checkpoint**: Assets directory ready with all required images

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: CSS architecture that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [x] T007 Create index.css at repository root with an initial :root scaffold for palette, semantic color aliases, Liquid Glass tokens, typography tokens, and layout tokens
- [x] T008 Add Metamatopoeia color palette CSS custom properties to index.css (--color-slate, --color-teal, --color-mist, --color-frost)
- [x] T009 Add Liquid Glass design tokens to index.css following --lg-{group}-{subgroup}-{token} naming convention
- [x] T010 Add responsive breakpoint and typography scale CSS custom properties to index.css using --lg-layout-_ and --lg-typography-_ token names
- [x] T011 Add base styles and reset to index.css (box-sizing, margins, html, body, img, a)
- [x] T012 Add layout classes to index.css (.layout-fullscreen, .layout-hero, .layout-list)
- [x] T013 Add card system styles to index.css (.card, .card-elevated)
- [x] T014 Add header and navigation styles to index.css (.header, .nav, .nav-brand, .nav-links, .nav-link)
- [x] T015 Add section styles to index.css (.section, .section-title, .section-hero)
- [x] T016 Add hero section styles to index.css (.hero-title, .hero-card, .hero-tagline)
- [x] T017 Add button styles to index.css (.button, .button-primary, .button-secondary)
- [x] T018 Add dark mode media query to index.css (@media prefers-color-scheme: dark)
- [x] T019 Add reduced motion media query to index.css (@media prefers-reduced-motion: reduce)
- [x] T020 Add reduced transparency media query to index.css (@media prefers-reduced-transparency: reduce)
- [x] T021 Add mobile responsive media query to index.css (@media max-width: 768px)

**Checkpoint**: CSS foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - First-Time Visitor Orientation (Priority: P1) 🎯 MVP

**Goal**: A visitor lands on the site and immediately understands what Metamatopoeia is, can navigate to see projects, and can reach out via a contact channel — all without leaving the page or triggering a network request.

**Independent Test**: Open `index.html` directly from the filesystem (no server). The page loads completely, all three sections are visible, navigation links scroll to the correct anchors, and all contact links resolve to valid `mailto:` or external URLs.

### Implementation for User Story 1

- [x] T022 Create index.html with HTML5 doctype, language attribute, and head section
- [x] T023 Add viewport meta tag and Metamatopoeia metadata (title, description, Open Graph) to index.html
- [x] T024 Link index.css stylesheet in index.html head section
- [x] T025 Add header navigation structure to index.html with brand name "Metamatopoeia" and three nav links (home, workshop, contact)
- [x] T026 Add main wrapper and Hero section (id="hero") to index.html with layout-fullscreen and layout-hero classes
- [x] T027 Add Hero content to index.html: brand name heading, .card-elevated with tagline "Human-Centered Software Intentionally Designed", CTA button linking to #workshop
- [x] T028 Add Workshop section (id="workshop") placeholder to index.html with layout-list class
- [x] T029 Add Contact section (id="contact") placeholder to index.html with layout-list class
- [x] T030 Add SpeedDial FAB placeholder structure to index.html (checkbox, label, actions container)
- [ ] T031 Test navigation scrolling: click nav links and verify smooth scroll to correct sections
- [ ] T032 Test zero network requests: open DevTools Network tab and verify zero external requests on page load

**Checkpoint**: At this point, User Story 1 should be fully functional and testable independently

---

## Phase 4: User Story 2 - Project Discovery in Workshop (Priority: P2)

**Goal**: A visitor browses the Workshop section to evaluate Metamatopoeia's project portfolio, reading project descriptions and following links to source code or live demos.

**Independent Test**: Navigate directly to `index.html#workshop`. All four project cards render with title, description, and at least one action button with a valid `href`.

### Implementation for User Story 2

- [x] T033 Add workshop CSS styles to index.css (.project-grid, .project-card, .project-image, .project-content, .project-title, .project-description, .project-actions)
- [x] T034 [P] Add Cup project card to index.html Workshop section with title, description, image, and three action buttons (Gist, Preview, Beta)
- [x] T035 [P] Add Liquid Glass UI project card to index.html Workshop section with title, description, image, and View Source Code button
- [x] T036 [P] Add Discover Breweries project card to index.html Workshop section with title, description, image, and View Source Code button
- [x] T037 [P] Add Assemble the Jams project card to index.html Workshop section with title, description, image, and View Source Code button
- [ ] T038 Test project cards render: verify all four cards display with images, titles, descriptions, and action buttons
- [ ] T039 Test project card links: click each action button and verify correct external destinations
- [ ] T040 Test mobile responsiveness: resize to <768px and verify cards stack vertically with legible text and tappable buttons

**Checkpoint**: At this point, User Stories 1 AND 2 should both work independently

---

## Phase 5: User Story 3 - Contact via SpeedDial FAB (Priority: P3)

**Goal**: A visitor uses the persistent floating SpeedDial FAB to quickly access any of the three Metamatopoeia contact channels from anywhere on the page.

**Independent Test**: With the page open, click the FAB. The three contact action buttons expand. Click each one and verify the correct destination. Press Escape — the FAB closes. Click outside the FAB — it closes.

### Implementation for User Story 3

- [x] T041 Add SpeedDial FAB CSS styles to index.css (.speed-dial-container, .speed-dial-checkbox, .speed-dial-fab, .speed-dial-icon, .speed-dial-actions, .speed-dial-action)
- [x] T042 Update SpeedDial FAB HTML in index.html with Email, GitHub, and LinkedIn action buttons with SVG icons and ARIA labels
- [x] T043 Add SpeedDial FAB JavaScript (no more than 30 non-empty source lines) to index.html: FAB toggle on click, close on action click, close on outside click, close on Escape key, keyboard navigation
- [ ] T044 Test FAB toggle: click FAB and verify three action buttons expand with correct icons
- [ ] T045 Test FAB close on action: click Email action and verify FAB closes and mailto: opens
- [ ] T046 Test FAB close on outside click: click outside FAB container and verify FAB closes
- [ ] T047 Test FAB close on Escape: press Escape key and verify FAB closes
- [ ] T048 Test keyboard navigation: Tab to FAB, press Enter, verify actions expand and are reachable by Tab

**Checkpoint**: At this point, User Stories 1, 2, AND 3 should all work independently

---

## Phase 6: User Story 4 - Contact Section (Priority: P3)

**Goal**: A visitor scrolls to or navigates to the Contact section and finds all three contact channels explicitly listed with clear labels and clickable links.

**Independent Test**: Navigate to `index.html#contact`. All three contact channels (email, GitHub, LinkedIn) are visible, labeled, and their links resolve correctly.

### Implementation for User Story 4

- [x] T049 Add contact section CSS styles to index.css (.contact-card, .contact-list, .contact-item, .contact-link, .contact-icon, .contact-label, .contact-value)
- [x] T050 Update Contact section in index.html with .card-elevated glass card layout
- [x] T051 Add Email contact channel to index.html Contact section with SVG icon, label, and mailto: link
- [x] T052 Add GitHub contact channel to index.html Contact section with SVG icon, label, and external link with rel="noopener noreferrer"
- [x] T053 Add LinkedIn contact channel to index.html Contact section with SVG icon, label, and external link with rel="noopener noreferrer"
- [ ] T054 Test contact section render: navigate to #contact and verify all three channels are visible with labels
- [ ] T055 Test contact links: click each channel and verify correct destinations (mailto: for email, new tab for GitHub/LinkedIn)

**Checkpoint**: At this point, User Stories 1, 2, 3, AND 4 should all work independently

---

## Phase 7: User Story 5 - Accessibility & Reduced-Motion (Priority: P2)

**Goal**: A visitor using assistive technology or a device with `prefers-reduced-motion` or `prefers-reduced-transparency` enabled experiences a fully usable, readable site.

**Independent Test**: Enable OS-level reduced motion. Reload page — all CSS transitions are disabled. Enable high-contrast/reduce-transparency — glass blur and opacity fall back gracefully. Tab through the entire page without a mouse — all interactive elements are reachable and visible.

### Implementation for User Story 5

- [x] T056 Add ARIA labels to all interactive elements in index.html (nav links, buttons, contact links, FAB)
- [x] T057 Add aria-hidden="true" to all decorative SVG icons in index.html
- [x] T058 Add :focus-visible styles to index.css for keyboard navigation focus indicators
- [ ] T059 Verify reduced motion works: enable OS reduced motion, reload page, verify --lg-glass-transition-duration is 0ms and scroll-behavior is auto
- [ ] T060 Verify reduced transparency works: enable OS reduced transparency, reload page, verify --lg-glass-bg-opacity is 0.98 and backdrop-filter is none
- [ ] T061 Test keyboard navigation: Tab through entire page without mouse, verify all interactive elements are reachable and focus indicators are visible
- [ ] T062 Test screen reader compatibility: verify decorative elements have aria-hidden="true" and interactive elements have accessible labels

**Checkpoint**: All user stories should now be independently functional with full accessibility support

---

## Phase 8: Polish & Cross-Cutting Concerns

**Purpose**: Final validation and quality assurance

- [ ] T063 Verify no personal names or biographical copy appear in visible text, metadata, headings, labels, or attribution in index.html or index.css
- [ ] T064 Verify all color values in index.css derive from the four palette tokens (search for hardcoded hex, rgb, hsl, named colors)
- [ ] T065 Verify the SpeedDial JavaScript block contains no more than 30 non-empty JavaScript source lines, excluding <script> tags and blank lines
- [ ] T066 Verify page renders without horizontal overflow at viewport widths from 280px to 1440px
- [ ] T067 Verify all project card images have descriptive alt text
- [ ] T068 Verify all external links have target="\_blank" and rel="noopener noreferrer"
- [ ] T069 Run quickstart.md validation checklist
- [ ] T070 Final browser test: open index.html from filesystem, test all user stories end-to-end
- [ ] T071 Verify missing project image fallback: simulate one failed project image load and confirm the card layout remains stable with descriptive alt text available
- [ ] T072 Verify very narrow viewport behavior at 280px, 320px, 375px, 768px, and 1440px with no horizontal overflow
- [ ] T073 Verify reduced motion and reduced transparency together: enable both preferences and confirm both overrides apply without conflict
- [ ] T074 Verify unsupported backdrop-filter fallback: disable backdrop-filter in DevTools and confirm glass surfaces remain readable through opaque token-derived backgrounds

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3-7)**: All depend on Foundational phase completion
  - User stories can then proceed in priority order (P1 → P2 → P3 → P2 → P3)
  - US1 (P1) is MVP and should be completed first
  - US2 (P2) and US5 (P2) can be done in parallel after US1
  - US3 (P3) and US4 (P3) can be done in parallel after US1
- **Polish (Phase 8)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories - MVP
- **User Story 2 (P2)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 3 (P3)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 4 (P3)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 5 (P2)**: Can start after Foundational (Phase 2) - No dependencies on other stories (cross-cutting)

**Note**: All user stories are independent and can be implemented in any order after foundational CSS is complete. Priority order is recommended for incremental delivery.

### Within Each User Story

- CSS styles before HTML structure
- HTML structure before testing
- Testing validates the story is complete

### Parallel Opportunities

- All Setup tasks marked [P] (T003-T006) can run in parallel (copying different images)
- All Foundational CSS tasks (T007-T021) are sequential (building the CSS foundation)
- All user stories can be worked on in parallel by different team members after Foundational phase
- Within US2, project card tasks (T034-T037) can run in parallel (different cards)
- Polish tasks (T063-T067) can run in parallel (different validation checks)

---

## Parallel Example: User Story 2

```bash
# Launch all project card creation tasks together:
Task: "Add Cup project card to index.html Workshop section with title, description, image, and three action buttons (Gist, Preview, Beta)"
Task: "Add Liquid Glass UI project card to index.html Workshop section with title, description, image, and View Source Code button"
Task: "Add Discover Breweries project card to index.html Workshop section with title, description, image, and View Source Code button"
Task: "Add Assemble the Jams project card to index.html Workshop section with title, description, image, and View Source Code button"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup (T001-T006)
2. Complete Phase 2: Foundational CSS (T007-T021) - CRITICAL
3. Complete Phase 3: User Story 1 (T022-T032)
4. **STOP and VALIDATE**: Test User Story 1 independently (open index.html, test navigation, verify zero network requests)
5. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently → Deploy/Demo (MVP!)
3. Add User Story 2 → Test independently → Deploy/Demo
4. Add User Story 3 → Test independently → Deploy/Demo
5. Add User Story 4 → Test independently → Deploy/Demo
6. Add User Story 5 → Test independently → Deploy/Demo
7. Complete Polish → Final validation
8. Each story adds value without breaking previous stories

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together
2. Once Foundational is done:
   - Developer A: User Story 1 (P1) - MVP priority
   - Developer B: User Story 2 (P2) - Project cards
   - Developer C: User Story 5 (P2) - Accessibility (cross-cutting)
3. After US1 complete:
   - Developer A: User Story 3 (P3) - SpeedDial FAB
   - Developer D: User Story 4 (P3) - Contact section
4. Stories complete and integrate independently

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Manual browser testing only (per constitution - no automated test framework)
- After each logical group, the human maintainer should review, stage, and commit changes from their own terminal; agents must not modify the Git index or history
- Stop at any checkpoint to validate story independently
- All colors MUST derive from the four palette tokens (constitution constraint)
- JavaScript MUST not exceed 30 non-empty source lines (constitution constraint)
- Zero external network requests (constitution constraint)
