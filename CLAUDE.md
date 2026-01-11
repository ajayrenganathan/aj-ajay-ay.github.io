# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal portfolio website for Ajay Renganathan, deployed via GitHub Pages at **iajayr.com**. This is a static single-page application with no build system.

## Development & Deployment

**No build commands required.** The site is pure HTML/CSS/JavaScript.

- **Deploy:** Push to `master` branch → GitHub Pages auto-deploys
- **Local preview:** Open `index.html` directly in a browser
- **Domain:** Configured via CNAME file to `iajayr.com`

## Architecture

**Single-file SPA:** Everything lives in `index.html` (~1,180 lines):
- Inline CSS in `<style>` tag
- Inline JavaScript in `<script>` tag
- No external dependencies except CDN-loaded Font Awesome and Google Fonts (Poppins)

**Page sections (in order):**
1. Fixed header with navigation (burger menu on mobile at 768px breakpoint)
2. Hero section with animated floating tech logos
3. About section
4. Skills section (3 cards)
5. Projects section (3 project cards with images)
6. Certifications section
7. Contact section with modal form

**JavaScript features:**
- Floating logo animation system with collision detection
- Smooth scroll navigation with ripple effects
- Contact form modal with validation
- Scroll progress indicator
- Mobile-responsive burger menu

## Assets

```
assets/images/
├── certifications/     # Certification badges
├── tech-logos/         # 14 technology logos (PNG/SVG)
├── project1.jpg        # Project showcase images
├── project2.jpg
└── project3.jpg
```

## Design System

- **Colors:** Primary (#4361ee), Secondary (#3a0ca3), Accent (#4cc9f0), Highlight (#f72585)
- **Font:** Poppins via Google Fonts
- **Mobile breakpoint:** 768px
- **Layouts:** CSS Grid and Flexbox
