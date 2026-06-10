# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static single-page portfolio website for Chelton, deployed on GitHub Pages at `https://chelton777.github.io/Chelton.portfolio/`. The entire site lives in a single `index.html` file — there is no build process, no Node.js, and no package manager.

## Development

**To preview locally**, open `index.html` directly in a browser or serve it with any static file server:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

There are no build, lint, or test commands — this project has no toolchain.

## Architecture

Everything is in `index.html` (≈700 lines), structured as:

1. `<head>` — meta/SEO tags, Open Graph, JSON-LD Person schema, CDN imports, Google Analytics 4 snippet
2. `<style>` — custom CSS for parallax, scroll animations, card hover effects, accessibility, and print styles
3. `<body>` sections (in order): fixed header nav → fixed social sidebar → `#home` hero → `#portfolio` grid → `#contact` form + info → footer
4. `<script>` — all vanilla JS: IntersectionObserver for scroll animations, parallax via `requestAnimationFrame`, form validation, EmailJS submission, GA4 event tracking

**No JavaScript framework or bundler is used.** DOM manipulation is plain JS.

## Styling Conventions

- **Tailwind CSS** is loaded via CDN (`v3.4.16`). Use Tailwind utility classes for layout, spacing, and typography.
- **Custom CSS** inside the `<style>` block handles things Tailwind cannot: `@keyframes`, parallax pseudo-elements (`::before`), `will-change` GPU hints, and `@media (prefers-reduced-motion)` overrides.
- Custom Tailwind theme (configured in the CDN `tailwind.config`):
  - `colors.primary` → `#333333`
  - `colors.secondary` → `#1a1a1a`
  - Custom `borderRadius` scale: `sm:4px` → `3xl:32px`
- The portfolio grid uses a custom CSS class `.portfolio-grid` (defined in `<style>`) rather than Tailwind grid utilities, to handle the 3→2→1 column responsive breakpoints.

## External Services & IDs

| Service | ID / Key | Purpose |
|---|---|---|
| Google Analytics 4 | `G-E2ELNGXC4B` | Page views, scroll depth, click events |
| EmailJS | service `service_vv7r3tl`, template `template_kb50h7r` | Contact form email delivery |
| Remix Icon CDN | v4.6.0 | All icons (`ri-*` classes) |
| Google Fonts | Inter (300–900) | Body and heading font |

EmailJS is initialized with a public key stored inline in the script. The contact form sends to that service on submit; do not remove or rename the form field `name` attributes (`user_name`, `user_phone`, `user_email`, `message`) as they map to the EmailJS template variables.

## GA4 Event Tracking

The script fires custom GA4 events for: social link clicks (`social_click`), CTA button clicks (`cta_click`), nav link clicks (`nav_click`), form submissions (`form_submit`) and errors (`form_error`), and scroll depth milestones (25/50/75/90%). Preserve these calls when modifying relevant elements.

## Deployment

Pushing to `main` publishes automatically to GitHub Pages — no CI workflow file is needed; GitHub Pages is configured to serve from the `main` branch root.
