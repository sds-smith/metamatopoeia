# HTML Structure Contract

**Feature**: Metamatopoeia Website  
**Date**: 2026-06-05  
**Purpose**: Define the semantic HTML structure for `index.html`

---

## Document Structure

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Metamatopoeia - Elegant Software Intentionally Designed</title>
    <meta
      name="description"
      content="Metamatopoeia: A software project showcase featuring elegant, intentionally designed applications."
    />
    <meta
      property="og:title"
      content="Metamatopoeia - Elegant Software Intentionally Designed"
    />
    <meta
      property="og:description"
      content="Metamatopoeia: A software project showcase featuring elegant, intentionally designed applications."
    />
    <meta property="og:type" content="website" />
    <link rel="stylesheet" href="index.css" />
  </head>
  <body>
    <!-- Header Navigation -->
    <header class="header">
      <nav class="nav">
        <div class="nav-brand">Metamatopoeia</div>
        <ul class="nav-links">
          <li>
            <a
              href="#hero"
              class="nav-link"
              aria-label="Navigate to Hero section"
              >home</a
            >
          </li>
          <li>
            <a
              href="#workshop"
              class="nav-link"
              aria-label="Navigate to Workshop section"
              >workshop</a
            >
          </li>
          <li>
            <a
              href="#contact"
              class="nav-link"
              aria-label="Navigate to Contact section"
              >contact</a
            >
          </li>
        </ul>
      </nav>
    </header>

    <!-- Main Content -->
    <main>
      <!-- Hero Section -->
      <section
        id="hero"
        class="section section-hero layout-fullscreen layout-hero"
      >
        <div class="hero-content">
          <h1 class="hero-title">Metamatopoeia</h1>
          <div class="card card-elevated hero-card">
            <p class="hero-tagline">Elegant Software Intentionally Designed</p>
            <a
              href="#workshop"
              class="button button-primary"
              aria-label="View our workshop projects"
              >Explore Workshop</a
            >
          </div>
        </div>
      </section>

      <!-- Workshop Section -->
      <section id="workshop" class="section section-workshop layout-list">
        <h2 class="section-title">Workshop</h2>
        <div class="project-grid">
          <!-- Project Card 1: Cup -->
          <article class="card card-elevated project-card">
            <img
              src="./assets/card-media-cup.png"
              alt="Cup project screenshot"
              class="project-image"
              loading="lazy"
            />
            <div class="project-content">
              <h3 class="project-title">Cup</h3>
              <p class="project-description">
                A decentralized social space for the specialty coffee community,
                built on the AT Protocol.
              </p>
              <div class="project-actions">
                <a
                  href="https://gist.github.com/sds-smith/10fe680d85823d0d3ee60045382f3c0b"
                  class="button button-secondary"
                  target="_blank"
                  rel="noopener noreferrer"
                  aria-label="View Cup Gist on GitHub"
                  >Check out the Gist</a
                >
                <a
                  href="https://cupsocial.app/"
                  class="button button-secondary"
                  target="_blank"
                  rel="noopener noreferrer"
                  aria-label="Visit Cup preview website"
                  >Check out the Preview</a
                >
                <a
                  href="https://cupsocial.app/"
                  class="button button-secondary"
                  target="_blank"
                  rel="noopener noreferrer"
                  aria-label="Join Cup beta"
                  >Join the Beta</a
                >
              </div>
            </div>
          </article>

          <!-- Project Card 2: Liquid Glass UI -->
          <article class="card card-elevated project-card">
            <img
              src="./assets/card-media-liquid-glass-ui.png"
              alt="Liquid Glass UI project screenshot"
              class="project-image"
              loading="lazy"
            />
            <div class="project-content">
              <h3 class="project-title">Liquid Glass UI</h3>
              <p class="project-description">
                A React component library for building interfaces with
                translucent, depth-aware surfaces inspired by the Liquid Glass
                design language.
              </p>
              <div class="project-actions">
                <a
                  href="https://github.com/metamatopoeia/liquid-glass-ui"
                  class="button button-secondary"
                  target="_blank"
                  rel="noopener noreferrer"
                  aria-label="View Liquid Glass UI source code on GitHub"
                  >View Source Code</a
                >
              </div>
            </div>
          </article>

          <!-- Project Card 3: Discover Breweries -->
          <article class="card card-elevated project-card">
            <img
              src="./assets/card-media-discover-breweries.png"
              alt="Discover Breweries project screenshot"
              class="project-image"
              loading="lazy"
            />
            <div class="project-content">
              <h3 class="project-title">Discover Breweries</h3>
              <p class="project-description">
                A web application for discovering and exploring craft breweries.
              </p>
              <div class="project-actions">
                <a
                  href="https://github.com/sds-smith/discover-breweries"
                  class="button button-secondary"
                  target="_blank"
                  rel="noopener noreferrer"
                  aria-label="View Discover Breweries source code on GitHub"
                  >View Source Code</a
                >
              </div>
            </div>
          </article>

          <!-- Project Card 4: Assemble the Jams -->
          <article class="card card-elevated project-card">
            <img
              src="./assets/card-media-atj.png"
              alt="Assemble the Jams project screenshot"
              class="project-image"
              loading="lazy"
            />
            <div class="project-content">
              <h3 class="project-title">Assemble the Jams</h3>
              <p class="project-description">
                A music curation and playlist management application.
              </p>
              <div class="project-actions">
                <a
                  href="https://github.com/sds-smith/assemble_the_jams_3"
                  class="button button-secondary"
                  target="_blank"
                  rel="noopener noreferrer"
                  aria-label="View Assemble the Jams source code on GitHub"
                  >View Source Code</a
                >
              </div>
            </div>
          </article>
        </div>
      </section>

      <!-- Contact Section -->
      <section id="contact" class="section section-contact layout-list">
        <h2 class="section-title">Contact</h2>
        <div class="card card-elevated contact-card">
          <ul class="contact-list">
            <li class="contact-item">
              <a
                href="mailto:metamatopoeia@gmail.com"
                class="contact-link"
                aria-label="Send email to metamatopoeia@gmail.com"
              >
                <svg
                  class="contact-icon"
                  aria-hidden="true"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <path
                    d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"
                  ></path>
                  <polyline points="22,6 12,13 2,6"></polyline>
                </svg>
                <span class="contact-label">Email</span>
                <span class="contact-value">metamatopoeia@gmail.com</span>
              </a>
            </li>
            <li class="contact-item">
              <a
                href="https://github.com/metamatopoeia"
                class="contact-link"
                target="_blank"
                rel="noopener noreferrer"
                aria-label="Visit Metamatopoeia on GitHub"
              >
                <svg
                  class="contact-icon"
                  aria-hidden="true"
                  viewBox="0 0 24 24"
                  fill="currentColor"
                >
                  <path
                    d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"
                  />
                </svg>
                <span class="contact-label">GitHub</span>
                <span class="contact-value">github.com/metamatopoeia</span>
              </a>
            </li>
            <li class="contact-item">
              <a
                href="https://www.linkedin.com/company/metamatopoeia"
                class="contact-link"
                target="_blank"
                rel="noopener noreferrer"
                aria-label="Visit Metamatopoeia on LinkedIn"
              >
                <svg
                  class="contact-icon"
                  aria-hidden="true"
                  viewBox="0 0 24 24"
                  fill="currentColor"
                >
                  <path
                    d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"
                  />
                </svg>
                <span class="contact-label">LinkedIn</span>
                <span class="contact-value"
                  >linkedin.com/company/metamatopoeia</span
                >
              </a>
            </li>
          </ul>
        </div>
      </section>
    </main>

    <!-- SpeedDial FAB -->
    <div class="speed-dial-container">
      <input
        type="checkbox"
        id="speed-dial-toggle"
        class="speed-dial-checkbox"
        aria-hidden="true"
      />
      <label
        for="speed-dial-toggle"
        class="speed-dial-fab"
        aria-label="Open contact menu"
        tabindex="0"
      >
        <svg
          class="speed-dial-icon"
          aria-hidden="true"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
        >
          <line x1="12" y1="5" x2="12" y2="19"></line>
          <line x1="5" y1="12" x2="19" y2="12"></line>
        </svg>
      </label>
      <div class="speed-dial-actions">
        <a
          href="mailto:metamatopoeia@gmail.com"
          class="speed-dial-action"
          aria-label="Send email to metamatopoeia@gmail.com"
        >
          <svg
            aria-hidden="true"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"
            ></path>
            <polyline points="22,6 12,13 2,6"></polyline>
          </svg>
        </a>
        <a
          href="https://github.com/metamatopoeia"
          class="speed-dial-action"
          target="_blank"
          rel="noopener noreferrer"
          aria-label="Visit Metamatopoeia on GitHub"
        >
          <svg aria-hidden="true" viewBox="0 0 24 24" fill="currentColor">
            <path
              d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"
            />
          </svg>
        </a>
        <a
          href="https://www.linkedin.com/company/metamatopoeia"
          class="speed-dial-action"
          target="_blank"
          rel="noopener noreferrer"
          aria-label="Visit Metamatopoeia on LinkedIn"
        >
          <svg aria-hidden="true" viewBox="0 0 24 24" fill="currentColor">
            <path
              d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"
            />
          </svg>
        </a>
      </div>
    </div>

    <script>
      (function () {
        const fab = document.querySelector(".speed-dial-fab");
        const checkbox = document.getElementById("speed-dial-toggle");
        const actions = document.querySelectorAll(".speed-dial-action");
        actions.forEach((action) => {
          action.addEventListener("click", () => {
            checkbox.checked = false;
          });
        });
        document.addEventListener("click", (e) => {
          if (!e.target.closest(".speed-dial-container")) {
            checkbox.checked = false;
          }
        });
        document.addEventListener("keydown", (e) => {
          if (e.key === "Escape") {
            checkbox.checked = false;
          }
        });
        fab.addEventListener("keydown", (e) => {
          if (e.key === "Enter" || e.key === " ") {
            e.preventDefault();
            checkbox.checked = !checkbox.checked;
          }
        });
      })();
    </script>
  </body>
</html>
```

---

## Semantic Requirements

- **Header**: `<header>` with `<nav>` for navigation
- **Main Content**: `<main>` wrapper for all sections
- **Sections**: `<section>` with `id` attributes for anchor navigation
- **Project Cards**: `<article>` for each project
- **Contact List**: `<ul>` with `<li>` for contact channels
- **Headings**: Hierarchical `<h1>`, `<h2>`, `<h3>` structure
- **Images**: `<img>` with `alt` text and `loading="lazy"`
- **Links**: `<a>` with appropriate `aria-label` and `rel` attributes
- **Decorative Elements**: SVG icons with `aria-hidden="true"`
- **Interactive Elements**: All have accessible labels

---

## Accessibility Requirements

- All interactive elements have `aria-label` where native semantics are insufficient
- Decorative SVGs have `aria-hidden="true"`
- External links have `target="_blank"` and `rel="noopener noreferrer"`
- Images have descriptive `alt` text
- Keyboard navigation is supported via `tabindex` and event handlers
- Focus indicators are handled via CSS (`:focus-visible`)

---

## Constraints

- No personal names in markup or content
- Exactly three contact channels (Email, GitHub, LinkedIn)
- Exactly four project cards
- Single `<script>` block at end of `<body>`
- JavaScript must not exceed 30 non-empty source lines, excluding `<script>` tags and blank lines
- No external CDN resources or network requests
- No `<iframe>` embeds
