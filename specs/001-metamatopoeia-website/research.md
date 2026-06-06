# Research: Reference Repository Analysis

**Feature**: Metamatopoeia Website  
**Date**: 2026-06-05  
**Reference**: sds-smith/html_portfolio (GitHub)

---

## Reference Repository Overview

The reference repository `sds-smith/html_portfolio` is a lightweight, responsive portfolio website built almost entirely with HTML and CSS, featuring liquid glass styling effects. It demonstrates modern CSS capabilities with minimal JavaScript usage compatible with the Metamatopoeia ≤30 non-empty source line cap.

### Architecture

**File Structure**:

- `index.html` - Main portfolio entry point
- `about.html` - Personal philosophy and values
- `portfolio.html` - Links to projects
- `index.css` - Single shared stylesheet
- `public/assets/` - Static assets (images)

**Key Characteristics**:

- Zero build tools or bundlers
- No JavaScript frameworks
- No CSS preprocessors
- No external dependencies or CDN resources
- Runs on vanilla HTML, CSS, and minimal JavaScript compatible with the ≤30 non-empty source line cap
- Zero external network requests

---

## Component System

### Card System

The reference uses a three-tier card system for glass morphism effects:

1. **`.card`** - Base glass morphism container
   - Backdrop blur
   - Transparency
   - Dynamic shadows

2. **`.card-elevated`** - Enhanced elevation with borders
   - Additional border styling
   - Increased shadow depth
   - Used for prominent content (hero tagline, project cards)

3. **`.card-outlined`** - Subtle outlined variant
   - Border-focused styling
   - Minimal blur
   - Used for secondary content

### Layout System

- **`.layout-fullscreen`** - Full viewport layouts for hero sections
- **`.layout-hero`** - Centered content display
- **`.layout-list`** - Vertical stacking for content sections
- Fully responsive grid and flexbox implementations

### Navigation

- Semantic header with responsive navigation
- Mobile-first responsive design
- No JavaScript required for basic navigation
- Sticky header with anchor-based scrolling

### Typography & Content

- System font stack for optimal performance and native appearance
- Responsive font sizing using `clamp()`
- Content truncation for mobile layouts
- Semantic HTML5 structure throughout

---

## Liquid Glass Design System

### Design Token System

The reference implements a comprehensive CSS custom property system for glass physics:

**Naming Convention**: `--lg-{group}-{subgroup}-{token}`

**Example Tokens**:

```css
--lg-glass-blur: 20px;
--lg-glass-bg-opacity: 0.12;
--lg-glass-border-opacity: 0.2;
--lg-glass-surface: rgb(var(--color-frost-rgb) / var(--lg-glass-bg-opacity));
--lg-glass-reflection: linear-gradient(
  135deg,
  rgb(var(--color-frost-rgb) / 0.4) 0%,
  rgb(var(--color-frost-rgb) / 0) 50%
);
```

### Glass Physics Implementation

- Backdrop blur with webkit fallbacks
- Layered box shadows for depth perception
- Gradient reflections for glass-like surfaces
- Smooth transitions with custom easing curves

### Accessibility Features

- Dark mode support via `prefers-color-scheme`
- Reduced motion support via `prefers-reduced-motion`
- Reduced transparency support via `prefers-reduced-transparency`
- Mobile-optimized glass effects
- Touch-friendly interaction targets
- Adaptive background handling

---

## JavaScript Usage

Only one component requires JavaScript: the SpeedDial floating action button.

**Functionality** (≤30 non-empty source lines):

- Checkbox-based state management
- Click-to-toggle functionality
- Outside click detection
- Keyboard navigation (Escape key)
- Action button interactions

**Characteristics**:

- Vanilla JavaScript (no frameworks)
- Framework-agnostic
- Progressively enhanced
- Single `<script>` block at bottom of `<body>`

---

## CSS Architecture

- CSS custom properties for theming
- Mobile-first responsive design
- Semantic class naming conventions
- Component-based organization
- Accessibility-first approach

---

## Performance Characteristics

- Zero build process required
- Instant loading with no external dependencies
- No network requests for fonts or frameworks
- Optimized for mobile devices
- Filesystem-openable (no server required)

---

## Browser Compatibility

- Modern CSS with graceful degradation
- Webkit prefixes for broader support
- Responsive images and layouts
- Touch-optimized interactions

---

## Key Adaptations for Metamatopoeia

### Structural Changes

1. **Single Page vs Multi-Page**: Reference has 3 pages (index, about, portfolio). Metamatopoeia will be a single-page with 3 sections (Hero, Workshop, Contact).
2. **Content Mapping**:
   - Reference `index.html` → Metamatopoeia Hero section
   - Reference `portfolio.html` → Metamatopoeia Workshop section
   - Reference About section → **EXCLUDED** (per constitution, no personal content)
   - New Contact section added (not in reference as dedicated section)

### Brand Adaptations

1. **Color Palette**: Reference uses unknown color scheme. Metamatopoeia MUST use the four-token palette (slate, teal, mist, frost).
2. **Branding**: Reference uses personal name "Shawn Smith". Metamatopoeia MUST use brand name only, no personal names.
3. **Contact Channels**: Reference contact channels unknown. Metamatopoeia MUST use exactly three authorized channels (email, GitHub, LinkedIn).

### Component Selection

1. **Keep**: `.card-elevated` (for hero tagline, project cards, contact section)
2. **Keep**: SpeedDial FAB (for contact access)
3. **Keep**: Sticky header navigation
4. **Keep**: `.layout-fullscreen`, `.layout-hero`, `.layout-list`
5. **Exclude**: AboutApp popover (contains personal copyright attribution)
6. **Exclude**: `.card` and `.card-outlined` (simplify to `.card-elevated` only for consistency)

### Asset Requirements

Per spec assumptions, the following assets will be copied from reference:

- Background image: `background-image-profile.jpeg` → `./assets/background-image-profile.jpeg`
- Card media images: From `public/assets/` → `./assets/`

---

## Technical Constraints Verified

✅ Zero build tools - matches reference  
✅ Single CSS file - matches reference  
✅ ≤30 non-empty source lines JavaScript - matches reference intent  
✅ Zero external network requests - matches reference  
✅ Semantic HTML5 - matches reference  
✅ Mobile-first responsive - matches reference  
✅ Accessibility support - matches reference  
✅ Liquid Glass design tokens - matches reference naming convention

---

## Conclusion

The reference repository `sds-smith/html_portfolio` is an excellent architectural match for the Metamatopoeia website requirements. The Liquid Glass design system, zero-dependency philosophy, and component structure align perfectly with the constitution constraints. The primary adaptations required are:

1. **Brand remapping**: Replace personal branding with Metamatopoeia brand
2. **Color palette**: Apply the four-token Metamatopoeia palette
3. **Structure consolidation**: Convert multi-page to single-page with three sections
4. **Content filtering**: Remove personal/biographical content, add dedicated Contact section
5. **Contact authorization**: Ensure only the three authorized contact channels are used

No technical barriers identified. The reference architecture can be directly adapted to meet all spec requirements.
