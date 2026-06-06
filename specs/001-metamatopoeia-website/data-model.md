# Data Model: Metamatopoeia Website

**Feature**: Metamatopoeia Website  
**Date**: 2026-06-05  
**Type**: Static HTML/CSS (no runtime data persistence)

---

## Overview

This is a static website with no backend, database, or runtime data persistence. All "data" is hardcoded in HTML markup. This document describes the semantic structure and content entities that will be represented in the markup.

---

## Content Entities

### Project Card

Represents a showcased software project in the Workshop section.

**Attributes**:

- `title` (string) - Project name
- `description` (string) - Short project description
- `media_image` (string) - Path to local asset image
- `actions` (array) - Action button objects
  - `label` (string) - Button text
  - `href` (string) - Link destination (GitHub repo, live demo, etc.)
  - `target` (string) - Link target (`_blank` for external, `_self` for internal)
  - `rel` (string) - Security attributes (`noopener noreferrer` for external)

**Instances** (4 total):

1. **Cup**
   - Title: "Cup"
   - Description: "A decentralized social space for the specialty coffee community, built on the AT Protocol."
   - Media Image: `./assets/card-media-cup.png` (from reference)
   - Actions:
     - "Check out the Gist" → `https://gist.github.com/sds-smith/10fe680d85823d0d3ee60045382f3c0b` (external)
     - "Check out the Preview" → `https://cupsocial.app/` (external)
     - "Join the Beta" → `https://cupsocial.app/` (external)

2. **Liquid Glass UI**
   - Title: "Liquid Glass UI"
   - Description: "A React component library for building interfaces with translucent, depth-aware surfaces inspired by the Liquid Glass design language."
   - Media Image: `./assets/card-media-liquid-glass-ui.png` (from reference)
   - Actions:
     - "View Source Code" → `https://github.com/metamatopoeia/liquid-glass-ui` (external)

3. **Discover Breweries**
   - Title: "Discover Breweries"
   - Description: "A web application for discovering and exploring craft breweries."
   - Media Image: `./assets/card-media-discover-breweries.png` (from reference)
   - Actions:
     - "View Source Code" → `https://github.com/sds-smith/discover-breweries` (external)

4. **Assemble the Jams**
   - Title: "Assemble the Jams"
   - Description: "A music curation and playlist management application."
   - Media Image: `./assets/card-media-atj.png` (from reference)
   - Actions:
     - "View Source Code" → `https://github.com/sds-smith/assemble_the_jams_3` (external)

**Note**: Characterization labels (Open-Source / Home Lab / Experimental) are deferred to v2 per FR-007.

---

### Contact Channel

Represents an authorized outreach method in the Contact section and SpeedDial FAB.

**Attributes**:

- `type` (enum) - Channel type: `email` | `github` | `linkedin`
- `label` (string) - Display label
- `href` (string) - Link destination
- `icon` (SVG) - Inline SVG icon for visual representation
- `aria_label` (string) - Accessible label for screen readers

**Instances** (3 total):

1. **Email**
   - Type: `email`
   - Label: "Email"
   - Href: `mailto:metamatopoeia@gmail.com`
   - Icon: Email envelope SVG
   - ARIA Label: "Send email to metamatopoeia@gmail.com"

2. **GitHub**
   - Type: `github`
   - Label: "GitHub"
   - Href: `https://github.com/metamatopoeia`
   - Icon: GitHub logo SVG
   - ARIA Label: "Visit Metamatopoeia on GitHub"

3. **LinkedIn**
   - Type: `linkedin`
   - Label: "LinkedIn"
   - Href: `https://www.linkedin.com/company/metamatopoeia`
   - Icon: LinkedIn logo SVG
   - ARIA Label: "Visit Metamatopoeia on LinkedIn"

---

### Navigation Item

Represents a link in the sticky header navigation.

**Attributes**:

- `label` (string) - Link text
- `href` (string) - In-page anchor destination
- `aria_label` (string) - Accessible label

**Instances** (3 total):

1. **Home**
   - Label: "home"
   - Href: `#hero`
   - ARIA Label: "Navigate to Hero section"

2. **Workshop**
   - Label: "workshop"
   - Href: `#workshop`
   - ARIA Label: "Navigate to Workshop section"

3. **Contact**
   - Label: "contact"
   - Href: `#contact`
   - ARIA Label: "Navigate to Contact section"

---

### Section

Represents a top-level content section on the page.

**Attributes**:

- `id` (string) - Section identifier for anchor navigation
- `title` (string) - Section heading (optional, for accessibility)
- `content` (mixed) - Section content (cards, text, etc.)

**Instances** (3 total):

1. **Hero**
   - ID: `hero`
   - Content: Brand name "Metamatopoeia", tagline card with "Human-Centered Software Intentionally Designed", CTA button linking to `#workshop`

2. **Workshop**
   - ID: `workshop`
   - Content: Four project cards (Cup, Liquid Glass UI, Discover Breweries, Assemble the Jams)

3. **Contact**
   - ID: `contact`
   - Content: Glass card displaying three contact channels (Email, GitHub, LinkedIn)

---

## Design Token Data

### Color Palette (CSS Custom Properties)

```css
:root {
  --color-slate: #5a606a; /* Primary dark / text */
  --color-teal: #79a1a2; /* Accent / interactive */
  --color-mist: #bdbfc6; /* Mid-tone / borders */
  --color-frost: #eff1f3; /* Light surface / bg */
  --color-slate-rgb: 90 96 106;
  --color-teal-rgb: 121 161 162;
  --color-mist-rgb: 189 191 198;
  --color-frost-rgb: 239 241 243;

  --lg-color-text-primary: var(--color-slate);
  --lg-color-text-muted: var(--color-mist);
  --lg-color-surface-page: var(--color-frost);
  --lg-color-accent-primary: var(--color-teal);
  --lg-color-accent-contrast: var(--color-frost);
}
```

### Liquid Glass Design Tokens (CSS Custom Properties)

```css
:root {
  /* Glass Physics */
  --lg-glass-blur: 20px;
  --lg-glass-bg-opacity: 0.12;
  --lg-glass-border-opacity: 0.2;
  --lg-glass-surface: rgb(var(--color-frost-rgb) / var(--lg-glass-bg-opacity));
  --lg-glass-border: rgb(
    var(--color-frost-rgb) / var(--lg-glass-border-opacity)
  );
  --lg-glass-reflection: linear-gradient(
    135deg,
    rgb(var(--color-frost-rgb) / 0.4) 0%,
    rgb(var(--color-frost-rgb) / 0) 50%
  );
  --lg-page-background-overlay: rgb(var(--color-slate-rgb) / 0.85);

  /* Transitions */
  --lg-glass-transition-duration: 300ms;
  --lg-glass-transition-easing: cubic-bezier(0.4, 0, 0.2, 1);

  /* Shadows */
  --lg-glass-shadow-sm: 0 2px 8px rgb(var(--color-slate-rgb) / 0.08);
  --lg-glass-shadow-md: 0 4px 16px rgb(var(--color-slate-rgb) / 0.12);
  --lg-glass-shadow-lg: 0 8px 32px rgb(var(--color-slate-rgb) / 0.16);
  --lg-glass-shadow-hover: 0 12px 40px rgb(var(--color-slate-rgb) / 0.2);
}
```

### Responsive Breakpoints

```css
:root {
  --lg-layout-breakpoint-mobile: 320px;
  --lg-layout-breakpoint-tablet: 768px;
  --lg-layout-breakpoint-desktop: 1024px;
}
```

### Typography Scale (clamp-based)

```css
:root {
  --lg-typography-size-xs: clamp(0.75rem, 0.7rem + 0.25vw, 0.875rem);
  --lg-typography-size-sm: clamp(0.875rem, 0.8rem + 0.375vw, 1rem);
  --lg-typography-size-base: clamp(1rem, 0.9rem + 0.5vw, 1.125rem);
  --lg-typography-size-lg: clamp(1.125rem, 1rem + 0.625vw, 1.25rem);
  --lg-typography-size-xl: clamp(1.25rem, 1.1rem + 0.75vw, 1.5rem);
  --lg-typography-size-2xl: clamp(1.5rem, 1.25rem + 1.25vw, 2rem);
  --lg-typography-size-3xl: clamp(2rem, 1.5rem + 2.5vw, 3rem);
}
```

---

## Media Query Overrides

### Dark Mode

```css
@media (prefers-color-scheme: dark) {
  --lg-color-text-primary: var(--color-frost);
  --lg-color-text-muted: var(--color-mist);
  --lg-color-surface-page: var(--color-slate);
  --lg-color-accent-primary: var(--color-teal);
  --lg-color-accent-contrast: var(--color-frost);
  --lg-page-background-overlay: rgb(var(--color-slate-rgb) / 0.9);
  --lg-glass-bg-opacity: 0.16; /* Slightly more opaque in dark mode */
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  --lg-glass-transition-duration: 0ms;
  html {
    scroll-behavior: auto;
  }
}
```

### Reduced Transparency

```css
@media (prefers-reduced-transparency: reduce) {
  --lg-glass-bg-opacity: 0.98;
  .card-elevated {
    backdrop-filter: none;
  }
}
```

---

## Component State

### SpeedDial FAB

**State**: Open / Closed

**State Management**:

- Checkbox-based state (CSS-only fallback)
- JavaScript enhancement for outside-click and Escape key handling
- Default state: Closed

**Transitions**:

- Open → Closed: Action buttons collapse, FAB icon rotates
- Closed → Open: Action buttons expand, FAB icon rotates

---

## Asset References

### Background Image

- Path: `./assets/background-image-profile.jpeg`
- Usage: Full-page background with rgba overlay
- Source: Reference repository `sds-smith/html_portfolio`

### Card Media Images

- Path: `./assets/card-media-*.png`
- Usage: Project card thumbnails
- Source: Reference repository `sds-smith/html_portfolio/public/assets/`

---

## Metadata

### HTML Document Metadata

```html
<title>Metamatopoeia - Human-Centered Software Intentionally Designed</title>
<meta
  name="description"
  content="Metamatopoeia: A software project showcase featuring elegant, intentionally designed applications."
/>
<meta
  property="og:title"
  content="Metamatopoeia - Human-Centered Software Intentionally Designed"
/>
<meta
  property="og:description"
  content="Metamatopoeia: A software project showcase featuring elegant, intentionally designed applications."
/>
<meta property="og:type" content="website" />
```

---

## Data Flow

Since this is a static site with no backend, there is no runtime data flow. All data is:

1. **Hardcoded** in HTML markup during authoring
2. **Styled** via CSS custom properties
3. **Interacted with** via vanilla JavaScript (SpeedDial FAB only)
4. **Persisted** only in the static files themselves

No API calls, no database queries, no client-side state management beyond the FAB toggle.
