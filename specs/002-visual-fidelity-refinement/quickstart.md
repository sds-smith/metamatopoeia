# Quickstart: 002 Layout Refinement

**Phase**: Phase 1 | **Date**: 2026-06-06

## Prerequisites

None. Zero-dependency, build-free static site. No package manager, no build tool, no server required.

## Opening the Page

```bash
# Direct filesystem open (no server needed):
xdg-open /workspaces/metamatopoeia/index.html

# OR use a local HTTP server for stable hot-reload dev:
python3 -m http.server 8080 --directory /workspaces/metamatopoeia
# Then open: http://localhost:8080
```

---

## Verification Checklist (against 11 SC- criteria)

After implementing each change group, verify the matching success criteria:

### SC-001 — Background right-justified [REQ-1, FR-001–FR-006]
1. Open at desktop viewport (≥1024px)
2. Background image visible on the **right side only** — does not fill full width
3. Left side shows only the gradient overlay fallback (no image bleed)
4. Resize to ≤768px → image switches to `cover` (fills full viewport)

### SC-002 — Header transparent [REQ-2, FR-007–FR-010]
1. Scroll down to Workshop section
2. Header bar has **no** visible background fill, blur, or bottom border
3. DevTools → Elements → `.header` → Computed → confirm `background`, `backdrop-filter` absent
4. Tab to nav links → focus indicators visible → links clickable

### SC-003/SC-004 — Workshop cards [REQ-3, FR-011–FR-020b]
1. Card images bleed **flush** to top and side card edges (no gap above image)
2. DevTools → Box Model on `.content` → padding = `16px` on all sides
3. Inspect DOM → action buttons are `<a class="button">` (no `<button>` inside `<a>`)
4. No `.project-image`, `.project-content`, `.project-actions`, `.project-description`, `.project-card`, `.project-grid` classes in DOM

### SC-005/SC-006 — Color tokens [REQ-4, FR-021–FR-023]
1. DevTools → Elements → select `<h1>` → Computed → `color`
   - Expected: `rgb(239, 241, 243)` (frost `#EFF1F3`)
2. DevTools → Elements → select `.card::after` pseudo-element → Computed → `background-image`
   - Expected: gradient stops containing `rgb(90, 96, 106 / ...)` (slate)
   - Must NOT contain `rgb(239, 241, 243 / ...)` (frost)

### SC-007 — FAB paper plane icon [REQ-5, FR-024–FR-025]
1. FAB displays a paper plane (not a "+") in closed state
2. Click FAB to open → icon **stays** as paper plane (no rotation, no swap)
3. Click FAB again or click outside → icon still paper plane
4. DevTools CSS → confirm no `.speed-dial-checkbox:checked ~ .speed-dial-fab .speed-dial-icon { transform: rotate(45deg); }` rule

### SC-008 — No console errors
1. DevTools → Console tab → hard reload
2. Zero errors, zero warnings

### SC-009 — Zero network requests
```
DevTools → Network tab → Disable cache → hard reload
- Confirm: zero external requests (no fonts, no CDN scripts, no remote images)
- All requests should be local file:// or localhost
```

### SC-010 — JS line count ≤30
```bash
# Count non-empty lines inside the <script> block:
awk '/<script>/{f=1;next} /<\/script>/{f=0} f && /\S/' \
  /workspaces/metamatopoeia/index.html | wc -l
# Expected output: ≤30
```

### SC-011 — No horizontal overflow at narrow viewports
1. DevTools → Toggle device toolbar → set width to 280px
2. No horizontal scrollbar appears
3. Layout is single-column, no content clipped

---

## File Reference

| File | Role | Lines (approx) | Edit? |
| --- | --- | --- | --- |
| `index.html` | Single HTML page | ~140 | YES — card HTML + FAB SVG |
| `index.css` | Single stylesheet | ~650 | YES — 19 CSS changes |
| `assets/` | Local image assets | N/A | NO |

## Change Summary (for quick diff review)

### `index.css` changes (~19 rules):
- `body` — remove background declarations
- `body::before` — new pseudo-element (right-justified bg)
- `@media (max-width: 768px)` — add `body::before { background-size: cover; }`
- `:root` — update `--lg-color-text-primary` and `--lg-glass-reflection`
- `.header` — remove glass surface properties; add `pointer-events: none`
- `.header *` — new rule: `pointer-events: auto`
- `.card` — change `::before` → `::after` (inset)
- `.card-elevated` — change `::before` → `::after`; remove `padding`; add `overflow: hidden`; update `border-radius`
- `.hero-card` — add `padding: 2rem`
- `.contact-card` — add `padding: 2rem`
- `.media` — new class
- `.content` — new class
- `.card-actions` — new class
- `.portfolio-links` — new class
- `.layout-list .card` — new rule: `width: 100%`
- `.button` — update to glass-surface style
- `.speed-dial-checkbox:checked ~ .speed-dial-fab .speed-dial-icon` — remove rotation rule
- `.project-grid`, `.project-card`, `.project-image`, `.project-content`, `.project-actions`, `.project-description` — delete all 6 rules

### `index.html` changes:
- 4 workshop cards: remap class attributes and wrapper div class
- FAB `<label>` inner SVG: replace icon
