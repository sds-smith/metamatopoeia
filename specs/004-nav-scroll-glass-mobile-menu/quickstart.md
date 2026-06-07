# Quickstart & Verification: 004 Nav Scroll-Glass & Mobile Hamburger Menu

**Phase**: 1 — Design  
**Generated**: 2026-06-07

---

## Prerequisites

- Open `index.html` directly in a browser (no server required — `file://` works).
- Use Chrome 115+, Firefox 110+, or Safari 17.4+ to verify all three features fully.
- Open DevTools → toggle device toolbar for mobile simulation.

---

## Acceptance Criteria

### AC-1: Scroll-Aware Nav Surface (Desktop + Mobile)

| #   | Step                                                            | Expected Result                                                                              |
| --- | --------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| 1   | Open page, do not scroll                                        | `.header` has no visible background, blur, or shadow — nav text floats over hero bg          |
| 2   | Scroll down 40px                                                | `.header` is partially transitioned — faint glass tint, partial blur                         |
| 3   | Scroll down to 80px+                                            | `.header` shows full glass surface: frosted background, backdrop-blur, bottom border, shadow |
| 4   | Scroll back to top                                              | `.header` returns to fully transparent                                                       |
| 5   | DevTools → Rendering → Emulate `prefers-reduced-motion: reduce` | Nav has a static glass surface (no animation), nav is not transparent                        |

**DevTools verification**: Elements panel → select `.header` → Computed styles → confirm `backdrop-filter` changes as you scroll.

---

### AC-2: Mobile Hamburger Menu

| #   | Step                                                             | Expected Result                                                                                               |
| --- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| 1   | Set viewport to 375px wide (mobile)                              | Hamburger icon (≡) appears on right of nav bar; nav links are hidden                                          |
| 2   | Click/tap hamburger icon                                         | Full-screen glass overlay slides in from top; nav links centered vertically; hamburger morphs to ✕            |
| 3   | Click/tap ✕ icon                                                 | Overlay slides back up; nav links hidden again; icon morphs back to ≡                                         |
| 4   | With menu closed, press Tab until the nav control receives focus | The visually hidden checkbox receives focus, and `.nav-hamburger` shows the visible focus outline             |
| 5   | With the nav control focused, press Space                        | Menu opens; hamburger morphs to ✕                                                                             |
| 6   | With menu open, continue pressing Tab                            | The 4 nav links become reachable in document order                                                            |
| 7   | With menu open, tap a nav link (e.g. "workshop")                 | Page scrolls to Workshop section; menu remains open (accepted CSS-only limitation — user must tap ✕ to close) |
| 8   | Close the menu and press Tab through the page again              | Nav links are NOT in tab order while the overlay is closed                                                    |

---

### AC-3: Nav-Brand Logo Mobile-Only

| #   | Step                       | Expected Result                                                                  |
| --- | -------------------------- | -------------------------------------------------------------------------------- |
| 1   | Mobile viewport (≤ 768px)  | `.nav-brand` shows logo image AND "Metamatopoeia" text side-by-side              |
| 2   | Desktop viewport (> 768px) | `.nav-brand` shows "Metamatopoeia" text; `<img class="nav-logo">` is NOT visible |

**DevTools verification**: Elements panel → select `img.nav-logo` → Computed → `display` should be `block` on mobile, `none` on desktop.

---

## Negative Cases

| #   | Scenario                                                   | Expected Behaviour                                                                                                                    |
| --- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| N-1 | Open in older browser without `animation-timeline` support | Nav stays permanently transparent; no broken layout; no console errors                                                                |
| N-2 | `prefers-reduced-transparency: reduce`                     | Glass `backdrop-filter` removed; solid background on scroll instead of frosted glass                                                  |
| N-3 | Keyboard nav with menu open                                | All 4 nav links reachable via Tab; focus is not trapped in the overlay (accepted CSS-only limitation); Escape key does NOT close menu |
| N-4 | Speed-dial FAB                                             | Unchanged; tapping FAB still opens contact links; JS block untouched                                                                  |
| N-5 | Closed mobile menu                                         | Closed overlay links are not reachable via Tab because `visibility: hidden` removes them from the tab order                           |

---

## Quick Smoke Test (copy-paste into browser console)

```js
// Verify JS line count has not exceeded budget (must stay ≤ 30)
const scriptContent = document.querySelector("script").textContent;
const nonEmptyLines = scriptContent
  .split("\n")
  .filter((l) => l.trim().length > 0).length;
console.assert(
  nonEmptyLines <= 30,
  `JS budget exceeded: ${nonEmptyLines} lines`,
);
console.log(`JS non-empty lines: ${nonEmptyLines} / 30`);
```

---

## File Change Summary

| File         | Lines Added                                                    | Lines Modified                                                 | Lines Removed             |
| ------------ | -------------------------------------------------------------- | -------------------------------------------------------------- | ------------------------- |
| `index.html` | ~6 (checkbox + label + 3 spans)                                | 0                                                              | 0                         |
| `index.css`  | ~110 (tokens, keyframes, hamburger, overlay, desktop override) | ~10 (header animation, nav-brand, nav-links, mobile-first nav) | ~4 (old mobile nav rules) |
