# Context Snapshot — Bug 005-nav-glass-blur-mobile

**Generated**: 2026-06-08 (Phase 4 complete)
**Purpose**: Bridge document for Phase 5 (SWE-1.6 session reset). Captures all nuance not yet in spec.md.

---

## Bug Summary

Nav glass scroll transition on mobile (iOS Chrome / WebKit): `backdrop-filter` blur intermittently fails to composite. Background tint applies; blur is absent. Failure is non-deterministic (random on same page load).

**Root cause confirmed**: WebKit does not reliably animate `backdrop-filter` on a `position: sticky` element when driven by `animation-timeline: scroll(root)`. This is a GPU compositor timing race.

---

## Fix Decision: `::before` Pseudo-Element Opacity Approach

**Chosen over**:
- `will-change: backdrop-filter` (hint only, not guaranteed)
- JS class-toggle (reliable but user preferred CSS-only; 4 JS lines remain but chosen not to use them)

**Mechanism**:
1. `.header::before` holds ALL glass surface properties as **static** values (not animated): `backdrop-filter`, `-webkit-backdrop-filter`, `background`, `box-shadow`, `border-bottom`.
2. Scroll-Driven Animation moves to `.header::before` and animates only `opacity: 0 → 1`.
3. `.header` loses all animation properties.
4. `@keyframes nav-glass-activate` is DELETED and replaced with `@keyframes nav-glass-surface-reveal` (opacity only).

---

## Key Architectural Decisions

### Decision 1: Static `backdrop-filter` on `::before`
`backdrop-filter` must NOT appear in a `@keyframes` block. Keeping it static eliminates the WebKit compositor promotion race. This is the core insight.

### Decision 2: `z-index: -1` on `::before`
`.header` creates a stacking context (position: sticky; z-index: 1000). `::before` at `z-index: -1` renders behind nav content (links, hamburger, brand) but within `.header`'s stacking context (i.e., still above scrolling page content). This is the correct layering.

### Decision 3: Reduced-motion scope expanded
User confirmed: the reduced-motion `.header` override (which applied inline glass properties directly to `.header`) MUST be removed and replaced with `.header::before { animation: none; opacity: 1; }` for architectural consistency. Glass state is exclusively owned by `::before` after this fix.

### Decision 4: Reduced-transparency scope updated
`.header` is removed from the `backdrop-filter: none` selector list in `prefers-reduced-transparency`. `.header::before` replaces it.

---

## Technical Risks Logged

### Risk 1: Scroll-Driven Animation on pseudo-elements in Safari 17.4
- `animation-timeline: scroll()` on `::before` must be supported in Safari 17.4+.
- Per CSS spec (Scroll-Driven Animations Level 1), pseudo-elements are valid animation targets.
- Safari 17.4 shipped Scroll-Driven Animations (March 2024). Pseudo-element support is expected.
- **Mitigation**: If verified unsupported, fallback is static transparent header at scroll top (same as older browser graceful degradation — not a regression from current state).

### Risk 2: `opacity: 0` + `backdrop-filter` bleed-through on WebKit
- Older WebKit (pre-2019) showed blur bleed-through at `opacity: 0`.
- Target browsers (Safari 17.4+) resolve this. Non-issue for stated browser support target.

---

## Files Changed (All in `index.css`)

| Location | Change |
|---|---|
| `.header` rule (HEADER & NAV section) | Remove 5 scroll animation properties |
| `@keyframes nav-glass-activate` block | Rename to `nav-glass-surface-reveal`; replace multi-property keyframe with single `opacity: 0 → 1` |
| After keyframe | Add `.header::before` rule (static glass + opacity animation) |
| `prefers-reduced-motion` block | Remove `.header` override (6 properties); add `.header::before { animation: none; opacity: 1; }` |
| `prefers-reduced-transparency` block | Replace `.header` with `.header::before` in selector list |

**No HTML changes. No JS changes. No new tokens.**

---

## Handoff Instructions for Phase 5 (SWE-1.6)

1. Read `spec.md` in `.specify/memory/` — it is the authoritative source.
2. Read this `context_snapshot.md` for architectural nuance.
3. Read `index.css` lines 295–315 (`.header` rule), 422–441 (`@keyframes nav-glass-activate`), 773–811 (`prefers-reduced-motion`), 813–826 (`prefers-reduced-transparency`) for exact current code.
4. Do NOT modify HTML. Do NOT modify JS. Do NOT add new CSS custom properties.
5. Run `/speckit.plan` to generate `plan.md` and `tasks.md` in `specs/005-nav-glass-blur-mobile/`.
