---
description: "Task list for 003-branding-and-founder-bridge"
---

# Tasks: Branding Enhancement & Founder Bridge

**Input**: `specs/003-branding-and-founder-bridge/plan.md`, `spec.md`  
**Branch**: `003-branding-and-founder-bridge`

---

## Phase 1: Foundation (Blocks All Subsequent Tasks)

**Purpose**: Ratify the Principle IV amendment before any HTML/CSS changes.

**⚠️ CRITICAL**: No user story work can begin until T001 and T001A are complete.

- [ ] T001 Amend `.specify/memory/constitution.md` to v2.1.0 — prepend Sync Impact Report comment, update Principle IV nav clause, bump version line to `2.1.0 | Last Amended: 2026-06-06`
- [ ] T001A STOP — user reviews, stages, and commits the constitution amendment from their own terminal, then confirms completion before T002 begins

**Checkpoint**: `constitution.md` version reads `2.1.0` and the user has confirmed the amendment commit. SC-011 satisfied. User story implementation may begin.

---

## Phase 2: User Story 4 — Background Logo Watermark (Priority: P2)

**Goal**: Add medium-opacity logo watermark to the left background via `body::after`.

**Why first in CSS**: Pure CSS, touches only `index.css`, zero HTML dependency. Safe to add and visually review before any HTML changes.

**Independent Test**: Open `index.html`. At ≥1024px viewport, teal logo mark is visible at ~45% opacity in the left background area. Network tab: zero external requests.

- [ ] T002 [US4] Add `body::after` rule to `index.css` immediately after the `body::before` block (Base Styles section):
  ```css
  body::after {
    content: "";
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: -1;
    pointer-events: none;
    background: url("./assets/Metamatopoeia_simple_transparent.png") left center
      no-repeat;
    background-size: auto 55vh;
    opacity: 0.45;
  }
  ```
- [ ] T003 [US4] Add watermark suppression inside the existing `@media (max-width: 768px)` block in `index.css`:
  ```css
  body::after {
    display: none;
  }
  ```

**Checkpoint**: Watermark visible at desktop; suppressed at ≤768px. No z-index conflicts with cards. Tune `55vh` / `0.45` as needed during visual review.

---

## Phase 3: User Story 1 — Nav Logo (Priority: P1) 🎯 MVP

**Goal**: Replace `.nav-brand` text with the Metamatopoeia logo image.

**Independent Test**: Open `index.html`. Nav-brand area shows the diamond mark image; text "Metamatopoeia" is no longer a visible text node in the header. `alt` text present for screen readers.

### CSS — User Story 1

- [ ] T004 [US1] Add `.nav-logo` rule to `index.css` inside the `/* HEADER & NAVIGATION */` section, after the `.nav-brand` block:
  ```css
  .nav-logo {
    display: block;
    height: 36px;
    width: auto;
  }
  ```

### HTML — User Story 1

- [ ] T005 [US1] In `index.html`, replace the text content of `.nav-brand` with the logo image:
  ```html
  <div class="nav-brand">
    <img
      src="./assets/Metamatopoeia_simple_small.png"
      alt="Metamatopoeia"
      class="nav-logo"
    />
  </div>
  ```

**Checkpoint**: Nav brand renders logo image at 36px height. Alt text "Metamatopoeia" present. Logo proportionally sized on mobile without nav overflow. SC-001 satisfied.

---

## Phase 4: User Story 3 — Nav "Meet the Founder" Link (Priority: P1)

**Goal**: Add a fourth nav link pointing to `https://sds-smith.io`.

**Independent Test**: Open `index.html`. Nav shows four links in order: home, workshop, meet the founder, contact. Clicking "meet the founder" opens `https://sds-smith.io` in a new tab.

### CSS — User Story 3

- [ ] T006 [US3] Add `flex-wrap: wrap` to `.nav-links` inside the existing `@media (max-width: 768px)` block in `index.css`, to prevent overflow with four links on mobile:
  ```css
  .nav-links {
    flex-wrap: wrap;
    justify-content: center;
  }
  ```

### HTML — User Story 3

- [ ] T007 [US3] In `index.html`, insert a new `<li>` into `.nav-links` between the "workshop" and "contact" items:
  ```html
  <li>
    <a
      href="https://sds-smith.io"
      class="nav-link"
      target="_blank"
      rel="noopener noreferrer"
      aria-label="Meet the Founder, visit personal website"
      >meet the founder</a
    >
  </li>
  ```

**Checkpoint**: Four nav links visible in correct order. "meet the founder" opens `https://sds-smith.io` in new tab. Mobile nav wraps cleanly. SC-005 satisfied.

---

## Phase 5: User Story 2 — Hero Asymmetric Dual-Text CTA (Priority: P1) 🎯 MVP

**Goal**: Remove the button CTA and replace with typographic text-link pair.

**Independent Test**: Open `index.html`. No `.button` or `.button-primary` in hero card. Two text links below tagline — "Scroll to Workshop" and "Meet the Founder" — separated by a `|` divider. Both links keyboard-navigable. "Scroll to Workshop" navigates to `#workshop`; "Meet the Founder" opens `https://sds-smith.io` in a new tab.

### CSS — User Story 2

- [ ] T008 [P] [US2] Add `.hero-cta`, `.hero-cta-link`, and `.hero-cta-divider` rules to `index.css` inside the `/* HERO SECTION */` section, after the `.hero-tagline` block:

  ```css
  .hero-cta {
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-top: 1.5rem;
    flex-wrap: wrap;
    justify-content: center;
  }

  .hero-cta-link {
    color: var(--lg-color-text-muted);
    font-size: var(--lg-typography-size-sm);
    text-decoration: none;
    transition: color var(--lg-glass-transition-duration)
      var(--lg-glass-transition-easing);
  }

  .hero-cta-link:hover,
  .hero-cta-link:focus-visible {
    color: var(--lg-color-text-primary);
  }

  .hero-cta-divider {
    color: var(--lg-color-text-muted);
    opacity: 0.4;
    user-select: none;
  }
  ```

### HTML — User Story 2

- [ ] T009 [US2] In `index.html`, inside `.hero-card`, remove the `<a class="button button-primary" ...>Explore Workshop</a>` element and replace with the `.hero-cta` structure:
  ```html
  <div class="hero-cta">
    <a
      href="#workshop"
      class="hero-cta-link"
      aria-label="Scroll to Workshop section"
      >Scroll to Workshop</a
    >
    <span class="hero-cta-divider" aria-hidden="true">|</span>
    <a
      href="https://sds-smith.io"
      class="hero-cta-link"
      target="_blank"
      rel="noopener noreferrer"
      aria-label="Meet the Founder, visit personal website"
      >Meet the Founder</a
    >
  </div>
  ```

**Checkpoint**: Hero card has no button shape. Two text links + divider render below tagline. Both links individually Tab-focusable. SC-002, SC-003, SC-004 satisfied.

---

## Phase 6: Polish & Verification

**Purpose**: Visual tune and full success criteria sweep.

- [ ] T010 Visual review — nav logo height: confirm 36px is proportionate; adjust to 32px or 40px if needed
- [ ] T011 Visual review — watermark: confirm `auto 55vh` size and `opacity: 0.45` read as "present but secondary"; tune ±5vh / ±0.05 if needed
- [ ] T012 Verify SC-001–SC-011 against `index.html` opened from filesystem (no server)
- [ ] T013 Confirm JS `<script>` block is ≤30 non-empty lines (expected: 13, unchanged)
- [ ] T014 DevTools Network tab: confirm zero external requests on cold load (cache disabled)
- [ ] T015 Responsive check: open DevTools, resize from 280px to 1440px — no horizontal overflow at any width

---

## Dependencies & Execution Order

```
T001 (constitution edit)
  └── T001A (user commit confirmation)
      └── T002 (body::after CSS)
      └── T003 (mobile watermark CSS)
      └── T004 (nav-logo CSS)  → T005 (nav-logo HTML)
      └── T006 (mobile nav-links CSS)  → T007 (nav 4th link HTML)
      └── T008 (hero-cta CSS)  → T009 (hero-cta HTML)
           └── T010–T015 (polish)
```

### Parallel Opportunities

- After T001A: T002, T003, T004, T006, T008 are all CSS additions to `index.css` and can be written together in one editing pass.
- T005, T007, T009 are three independent HTML edits that can be done in a single `index.html` editing pass after all CSS is in place.
- T010–T015 (polish/verification) can be run concurrently after T009.

### Recommended Execution

1. **T001** — edit constitution amendment
2. **T001A** — stop for user review, staging, commit, and confirmation
3. **T002–T004 + T006 + T008** — single `index.css` editing pass (all CSS additions)
4. **T005 + T007 + T009** — single `index.html` editing pass (all HTML changes)
5. **T010–T015** — visual review and verification sweep
