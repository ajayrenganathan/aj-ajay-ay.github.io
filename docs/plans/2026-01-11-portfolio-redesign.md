# Portfolio Redesign Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Transform the portfolio into a refined, dual-theme website reflecting "simplicity is the key to scaling" — warm minimalist and dark terminal themes with subtle animations.

**Architecture:** Single-file static site (index.html) with all CSS as custom properties for easy theme swapping. Theme toggle persists to localStorage. Animations are CSS-only where possible, minimal JS for theme switching and scroll behaviors.

**Tech Stack:** HTML5, CSS3 (custom properties, grid, transitions), Vanilla JS, Google Fonts (Instrument Serif, Source Sans 3, JetBrains Mono), Font Awesome

---

## Task 1: CSS Foundation — Color System, Typography & Texture

**Files:**
- Modify: `index.html` (style section, lines 8-780)

**Step 1: Replace CSS custom properties with new theme system**

Replace the existing `:root` block (lines 9-16) with:

```css
:root {
    /* Warm Minimalist Theme (default) */
    --bg-primary: #FAF9F7;
    --bg-secondary: #FFFFFF;
    --bg-tertiary: #F5F4F2;
    --text-primary: #2D2A26;
    --text-secondary: #5C5954;
    --text-muted: #8B8680;
    --accent: #C17F59;
    --accent-hover: #A66B48;
    --accent-subtle: rgba(193, 127, 89, 0.1);
    --border: rgba(45, 42, 38, 0.1);
    --shadow: rgba(45, 42, 38, 0.08);
    --shadow-hover: rgba(45, 42, 38, 0.15);
    --logo-opacity: 0.06;
    --grain-opacity: 0.018;

    /* Typography */
    --font-display: 'Instrument Serif', Georgia, serif;
    --font-body: 'Source Sans 3', -apple-system, sans-serif;
    --font-mono: 'JetBrains Mono', monospace;

    /* Spacing */
    --section-padding: 6rem;
    --container-width: 900px;
    --card-radius: 8px;

    /* Transitions */
    --transition-fast: 0.2s ease;
    --transition-medium: 0.3s ease;
    --transition-slow: 0.5s ease;
}

[data-theme="dark"] {
    --bg-primary: #1A1B1E;
    --bg-secondary: #242529;
    --bg-tertiary: #2A2B30;
    --text-primary: #E8E6E3;
    --text-secondary: #B8B5B0;
    --text-muted: #6B6E76;
    --accent: #7AA2AB;
    --accent-hover: #8FB5BE;
    --accent-subtle: rgba(122, 162, 171, 0.15);
    --border: rgba(232, 230, 227, 0.1);
    --shadow: rgba(0, 0, 0, 0.2);
    --shadow-hover: rgba(0, 0, 0, 0.35);
    --logo-opacity: 0.08;
    --grain-opacity: 0.012;
}
```

**Step 2: Add Google Fonts import**

Add after line 7 (before `<style>`):

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=JetBrains+Mono:wght@400;500&family=Source+Sans+3:wght@400;500;600&display=swap" rel="stylesheet">
```

**Step 3: Update base styles with subtle grain texture**

Replace the `*` and `body` rules (lines 18-31) with:

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: var(--font-body);
    background-color: var(--bg-primary);
    color: var(--text-primary);
    line-height: 1.7;
    overflow-x: hidden;
    scroll-behavior: smooth;
    transition: background-color var(--transition-medium), color var(--transition-medium);
}

/* Subtle grain texture for tactile warmth */
body::before {
    content: '';
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 9999;
    opacity: var(--grain-opacity);
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
}
```

**Step 4: Verify in browser**

Open `index.html` in browser. Page should display with warm off-white background, charcoal text, and a very subtle grain texture overlay.

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add dual-theme color system, typography, and grain texture"
```

---

## Task 2: Header & Theme Toggle with Animation

**Files:**
- Modify: `index.html` (style section + header HTML + script section)

**Step 1: Replace header styles**

Remove existing header and navbar styles (approximately lines 49-152) and replace with:

```css
header {
    position: fixed;
    top: 0;
    width: 100%;
    z-index: 100;
    background-color: var(--bg-primary);
    transition: background-color var(--transition-medium), box-shadow var(--transition-medium);
}

header.scrolled {
    box-shadow: 0 1px 20px var(--shadow);
    backdrop-filter: blur(10px);
    background-color: rgba(250, 249, 247, 0.9);
}

[data-theme="dark"] header.scrolled {
    background-color: rgba(26, 27, 30, 0.9);
}

.navbar {
    max-width: var(--container-width);
    margin: 0 auto;
    padding: 1.25rem 2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.logo {
    font-family: var(--font-display);
    font-size: 1.5rem;
    font-weight: 400;
    color: var(--text-primary);
    text-decoration: none;
}

.nav-right {
    display: flex;
    align-items: center;
    gap: 2.5rem;
}

.nav-links {
    display: flex;
    list-style: none;
    gap: 2rem;
}

.nav-links a {
    font-family: var(--font-body);
    font-size: 0.9rem;
    font-weight: 500;
    color: var(--text-secondary);
    text-decoration: none;
    transition: color var(--transition-fast);
    position: relative;
}

.nav-links a::after {
    content: '';
    position: absolute;
    left: 0;
    bottom: -4px;
    width: 0;
    height: 1.5px;
    background-color: var(--accent);
    transition: width var(--transition-fast);
}

.nav-links a:hover,
.nav-links a.active {
    color: var(--text-primary);
}

.nav-links a:hover::after,
.nav-links a.active::after {
    width: 100%;
}

/* Theme toggle with animated icon transition */
.theme-toggle {
    background: none;
    border: 1.5px solid var(--border);
    border-radius: 50%;
    width: 40px;
    height: 40px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--text-secondary);
    transition: all var(--transition-fast);
    position: relative;
    overflow: hidden;
}

.theme-toggle:hover {
    border-color: var(--accent);
    color: var(--accent);
}

.theme-toggle i {
    transition: transform var(--transition-medium), opacity var(--transition-fast);
    position: absolute;
}

.theme-toggle:hover i {
    transform: rotate(15deg);
}

.theme-toggle .fa-moon {
    opacity: 1;
    transform: rotate(0deg);
}

.theme-toggle .fa-sun {
    opacity: 0;
    transform: rotate(-90deg);
}

[data-theme="dark"] .theme-toggle .fa-moon {
    opacity: 0;
    transform: rotate(90deg);
}

[data-theme="dark"] .theme-toggle .fa-sun {
    opacity: 1;
    transform: rotate(0deg);
}

[data-theme="dark"] .theme-toggle:hover .fa-sun {
    transform: rotate(15deg);
}

/* Mobile Navigation */
.burger-menu {
    display: none;
    flex-direction: column;
    cursor: pointer;
    gap: 5px;
}

.burger-bar {
    width: 22px;
    height: 2px;
    background-color: var(--text-primary);
    transition: var(--transition-fast);
}

@media (max-width: 768px) {
    .burger-menu {
        display: flex;
    }

    .nav-links {
        display: none;
        position: fixed;
        top: 70px;
        left: 0;
        right: 0;
        background-color: var(--bg-primary);
        flex-direction: column;
        padding: 2rem;
        gap: 1.5rem;
        box-shadow: 0 10px 30px var(--shadow);
    }

    .nav-links.active {
        display: flex;
    }

    .nav-right {
        gap: 1rem;
    }
}
```

**Step 2: Update header HTML**

Replace the header section (around lines 1038-1056) with:

```html
<header>
    <nav class="navbar">
        <a href="#" class="logo">Ajay Renganathan</a>
        <div class="nav-right">
            <div class="burger-menu">
                <div class="burger-bar"></div>
                <div class="burger-bar"></div>
                <div class="burger-bar"></div>
            </div>
            <ul class="nav-links">
                <li><a href="#about">About</a></li>
                <li><a href="#skills">Skills</a></li>
                <li><a href="#projects">Projects</a></li>
                <li><a href="#certifications">Certifications</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
            <button class="theme-toggle" aria-label="Toggle theme">
                <i class="fas fa-moon"></i>
                <i class="fas fa-sun"></i>
            </button>
        </div>
    </nav>
</header>
```

**Step 3: Add theme toggle JavaScript**

Add at the beginning of the script section (after `document.addEventListener('DOMContentLoaded', () => {`):

```javascript
// Theme Toggle
const themeToggle = document.querySelector('.theme-toggle');
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
const storedTheme = localStorage.getItem('theme');

if (storedTheme) {
    document.documentElement.setAttribute('data-theme', storedTheme);
} else if (prefersDark) {
    document.documentElement.setAttribute('data-theme', 'dark');
}

themeToggle.addEventListener('click', () => {
    const currentTheme = document.documentElement.getAttribute('data-theme');
    const newTheme = currentTheme === 'dark' ? 'light' : 'dark';

    if (newTheme === 'light') {
        document.documentElement.removeAttribute('data-theme');
    } else {
        document.documentElement.setAttribute('data-theme', 'dark');
    }
    localStorage.setItem('theme', newTheme);
});

// Header scroll effect
const header = document.querySelector('header');
window.addEventListener('scroll', () => {
    if (window.scrollY > 50) {
        header.classList.add('scrolled');
    } else {
        header.classList.remove('scrolled');
    }
});

// Active nav link on scroll
const sections = document.querySelectorAll('section[id]');
const navLinks = document.querySelectorAll('.nav-links a');

window.addEventListener('scroll', () => {
    let current = '';
    sections.forEach(section => {
        const sectionTop = section.offsetTop - 100;
        if (scrollY >= sectionTop) {
            current = section.getAttribute('id');
        }
    });

    navLinks.forEach(link => {
        link.classList.remove('active');
        if (link.getAttribute('href') === `#${current}`) {
            link.classList.add('active');
        }
    });
});
```

**Step 4: Verify in browser**

- Header should be clean with warm background
- Click theme toggle — icons should rotate/morph smoothly between sun and moon
- Hover on toggle — icon should rotate 15 degrees
- Refresh page — theme should persist
- Scroll down — header should get subtle shadow

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add minimal header with animated theme toggle"
```

---

## Task 3: Hero Section — Typography & Subtle Tech Backdrop

**Files:**
- Modify: `index.html` (style section + hero HTML + script section)

**Step 1: Replace hero styles**

Remove existing hero, floating-logos, and tech-logo styles. Replace with:

```css
.hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    padding: 8rem 2rem 4rem;
    overflow: hidden;
}

.hero-backdrop {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 0;
}

.tech-logo {
    position: absolute;
    width: 48px;
    height: 48px;
    opacity: var(--logo-opacity);
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
    filter: grayscale(100%);
    transition: opacity var(--transition-slow);
}

.hero-content {
    position: relative;
    z-index: 1;
    text-align: center;
    max-width: 700px;
}

.hero-content h1 {
    font-family: var(--font-display);
    font-size: clamp(2.5rem, 6vw, 4rem);
    font-weight: 400;
    color: var(--text-primary);
    margin-bottom: 1.5rem;
    line-height: 1.2;
}

.hero-tagline {
    font-size: 1.25rem;
    color: var(--text-secondary);
    margin-bottom: 2.5rem;
    font-weight: 400;
}

.hero-cta {
    display: inline-block;
    font-family: var(--font-body);
    font-size: 0.95rem;
    font-weight: 500;
    color: var(--bg-primary);
    background-color: var(--text-primary);
    padding: 0.9rem 2rem;
    border-radius: 4px;
    text-decoration: none;
    transition: all var(--transition-fast);
}

.hero-cta:hover {
    background-color: var(--accent);
    transform: translateY(-2px);
}

.scroll-prompt {
    position: absolute;
    bottom: 3rem;
    left: 50%;
    transform: translateX(-50%);
    color: var(--text-muted);
    cursor: pointer;
    animation: gentleBounce 3s ease-in-out infinite;
}

.scroll-prompt i {
    font-size: 1.25rem;
}

@keyframes gentleBounce {
    0%, 100% { transform: translateX(-50%) translateY(0); }
    50% { transform: translateX(-50%) translateY(8px); }
}
```

**Step 2: Update hero HTML**

Replace the hero section with (tightened tagline — let "curious" show through the work):

```html
<section class="hero">
    <div class="hero-backdrop" id="heroBackdrop"></div>
    <div class="hero-content">
        <h1>Ajay Renganathan</h1>
        <p class="hero-tagline">Software architect. Perpetually curious.<br>Building simple systems for complex problems.</p>
        <a href="#contact" class="hero-cta">Let's Build Something</a>
    </div>
    <div class="scroll-prompt" onclick="document.getElementById('about').scrollIntoView({ behavior: 'smooth' });">
        <i class="fas fa-chevron-down"></i>
    </div>
</section>
```

**Step 3: Replace floating logos JavaScript**

Replace the entire techLogos and createAnimatedLogo logic with:

```javascript
// Subtle Tech Backdrop
const techLogos = [
    'python.png', 'javascript.png', 'aws.png', 'docker.png',
    'kubernetes.png', 'mongodb.png', 'postgresql.png', 'elasticsearch.png',
    'terraform.png', 'flask.png', 'django.png', 'fastapi.svg',
    'kafka.png', 'github.png'
];

function createSubtleBackdrop() {
    const backdrop = document.getElementById('heroBackdrop');
    if (!backdrop) return;

    techLogos.forEach((logo, index) => {
        const el = document.createElement('div');
        el.className = 'tech-logo';
        el.style.backgroundImage = `url('assets/images/tech-logos/${logo}')`;

        // Distribute across the viewport
        const col = index % 5;
        const row = Math.floor(index / 5);
        const xBase = (col * 20) + 5 + (Math.random() * 10);
        const yBase = (row * 30) + 15 + (Math.random() * 15);

        el.style.left = `${xBase}%`;
        el.style.top = `${yBase}%`;

        // Very slow drift animation
        const duration = 40 + Math.random() * 30; // 40-70 seconds
        const xDrift = 20 + Math.random() * 30;
        const yDrift = 15 + Math.random() * 25;

        el.animate([
            { transform: 'translate(0, 0)' },
            { transform: `translate(${xDrift}px, ${yDrift}px)` }
        ], {
            duration: duration * 1000,
            iterations: Infinity,
            direction: 'alternate',
            easing: 'ease-in-out'
        });

        backdrop.appendChild(el);
    });
}

createSubtleBackdrop();
```

**Step 4: Remove old collision detection and related code**

Delete the collision detection `setInterval` and all hover handlers from the old logo system.

**Step 5: Verify in browser**

- Hero should show name prominently
- Tagline reads: "Software architect. Perpetually curious." + "Building simple systems for complex problems."
- Tech logos should be barely visible, drifting very slowly
- Toggle theme — logos should be slightly more visible in dark mode
- Overall feel: calm, not chaotic

**Step 6: Commit**

```bash
git add index.html
git commit -m "feat: redesign hero with typography focus and subtle tech backdrop"
```

---

## Task 4: Section Base Styles & About Section with Distinctive Pull Quote

**Files:**
- Modify: `index.html` (style section + about HTML)

**Step 1: Add section base styles with distinctive pull quote**

```css
.section {
    padding: var(--section-padding) 2rem;
}

.section:nth-child(even) {
    background-color: var(--bg-secondary);
}

.container {
    max-width: var(--container-width);
    margin: 0 auto;
}

.section-header {
    margin-bottom: 3rem;
}

.section-title {
    font-family: var(--font-display);
    font-size: 2rem;
    font-weight: 400;
    color: var(--text-primary);
    margin-bottom: 0.75rem;
}

.section-title::after {
    content: '';
    display: block;
    width: 40px;
    height: 2px;
    background-color: var(--accent);
    margin-top: 0.75rem;
    opacity: 0.6;
}

.section-content p {
    color: var(--text-secondary);
    margin-bottom: 1.25rem;
    max-width: 65ch;
}

.section-content p:last-child {
    margin-bottom: 0;
}

/* Fade-in on scroll */
.fade-in {
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.6s ease, transform 0.6s ease;
}

.fade-in.visible {
    opacity: 1;
    transform: translateY(0);
}

/* Distinctive centered pull quote with decorative marks */
.pull-quote {
    font-family: var(--font-display);
    font-style: italic;
    font-size: 1.4rem;
    color: var(--accent);
    text-align: center;
    padding: 2.5rem 1rem;
    margin: 2.5rem 0;
    position: relative;
    line-height: 1.4;
}

.pull-quote::before {
    content: '"';
    position: absolute;
    top: 0;
    left: 50%;
    transform: translateX(-50%);
    font-family: var(--font-display);
    font-size: 4rem;
    color: var(--accent);
    opacity: 0.15;
    line-height: 1;
}

.pull-quote::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 60px;
    height: 2px;
    background-color: var(--accent);
    opacity: 0.3;
}
```

**Step 2: Update About section HTML**

```html
<section id="about" class="section fade-in">
    <div class="container">
        <div class="section-header">
            <h2 class="section-title">About</h2>
        </div>
        <div class="section-content">
            <p>
                I'm a software architect and engineer with a deep curiosity for how systems work at scale. My expertise spans cloud architecture, DevOps practices, and building robust distributed systems.
            </p>
            <p>
                I believe the best solutions are often the simplest ones. Complex problems don't require complex solutions — they require thoughtful decomposition and elegant design.
            </p>
            <blockquote class="pull-quote">
                Simplicity is the key to scaling.
            </blockquote>
            <p>
                When I'm not architecting systems, I'm tinkering with new technologies, exploring infrastructure patterns, and finding better ways to automate the mundane.
            </p>
        </div>
    </div>
</section>
```

**Step 3: Add scroll fade-in JavaScript**

```javascript
// Fade in on scroll
const fadeElements = document.querySelectorAll('.fade-in');

const fadeObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add('visible');
        }
    });
}, { threshold: 0.1 });

fadeElements.forEach(el => fadeObserver.observe(el));
```

**Step 4: Verify in browser**

- About section should fade in when scrolled into view
- Pull quote should be centered with large decorative opening quote mark
- Pull quote has subtle underline accent
- Text should be readable and well-spaced

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add section base styles with distinctive pull quote design"
```

---

## Task 5: Social Proof Section — GitHub & Testimonials

**Files:**
- Modify: `index.html` (style section + new section HTML)

**Step 1: Add social proof styles**

```css
.social-proof {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    align-items: start;
}

/* GitHub Stats Card */
.github-card {
    background-color: var(--bg-secondary);
    border: 1px solid var(--border);
    border-radius: var(--card-radius);
    padding: 2rem;
    transition: all var(--transition-medium);
}

.section:nth-child(even) .github-card {
    background-color: var(--bg-primary);
}

.github-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 30px var(--shadow-hover);
}

.github-header {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    margin-bottom: 1.5rem;
}

.github-header i {
    font-size: 1.5rem;
    color: var(--text-primary);
}

.github-header h3 {
    font-family: var(--font-body);
    font-size: 1.1rem;
    font-weight: 600;
    color: var(--text-primary);
}

.github-stats {
    display: flex;
    gap: 2rem;
    margin-bottom: 1.5rem;
}

.stat {
    text-align: center;
}

.stat-value {
    font-family: var(--font-mono);
    font-size: 1.5rem;
    font-weight: 500;
    color: var(--accent);
    display: block;
}

.stat-label {
    font-size: 0.8rem;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.5px;
}

.github-repos {
    border-top: 1px solid var(--border);
    padding-top: 1rem;
}

.github-repos h4 {
    font-size: 0.85rem;
    font-weight: 500;
    color: var(--text-muted);
    margin-bottom: 0.75rem;
}

.repo-list {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
}

.repo-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.5rem 0;
    border-bottom: 1px dashed var(--border);
}

.repo-item:last-child {
    border-bottom: none;
}

.repo-name {
    font-family: var(--font-mono);
    font-size: 0.85rem;
    color: var(--text-primary);
    text-decoration: none;
    transition: color var(--transition-fast);
}

.repo-name:hover {
    color: var(--accent);
}

.repo-stars {
    display: flex;
    align-items: center;
    gap: 0.3rem;
    font-size: 0.8rem;
    color: var(--text-muted);
}

.repo-stars i {
    color: var(--accent);
    font-size: 0.75rem;
}

/* Testimonial Card */
.testimonial-card {
    background-color: var(--bg-secondary);
    border: 1px solid var(--border);
    border-radius: var(--card-radius);
    padding: 2rem;
    position: relative;
    transition: all var(--transition-medium);
}

.section:nth-child(even) .testimonial-card {
    background-color: var(--bg-primary);
}

.testimonial-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 30px var(--shadow-hover);
}

.testimonial-card::before {
    content: '"';
    position: absolute;
    top: 1rem;
    left: 1.5rem;
    font-family: var(--font-display);
    font-size: 4rem;
    color: var(--accent);
    opacity: 0.15;
    line-height: 1;
}

.testimonial-text {
    font-size: 1rem;
    color: var(--text-secondary);
    line-height: 1.7;
    margin-bottom: 1.5rem;
    font-style: italic;
}

.testimonial-author {
    display: flex;
    align-items: center;
    gap: 1rem;
}

.author-avatar {
    width: 48px;
    height: 48px;
    border-radius: 50%;
    background-color: var(--accent-subtle);
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: var(--font-display);
    font-size: 1.25rem;
    color: var(--accent);
}

.author-info h4 {
    font-size: 0.95rem;
    font-weight: 600;
    color: var(--text-primary);
}

.author-info p {
    font-size: 0.8rem;
    color: var(--text-muted);
}

.github-link {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    margin-top: 1rem;
    font-size: 0.9rem;
    color: var(--accent);
    text-decoration: none;
    transition: color var(--transition-fast);
}

.github-link:hover {
    color: var(--accent-hover);
}
```

**Step 2: Add Social Proof section HTML**

Insert after the About section:

```html
<section id="social-proof" class="section fade-in">
    <div class="container">
        <div class="section-header">
            <h2 class="section-title">Building in Public</h2>
        </div>
        <div class="social-proof">
            <div class="github-card">
                <div class="github-header">
                    <i class="fab fa-github"></i>
                    <h3>GitHub Activity</h3>
                </div>
                <div class="github-stats">
                    <div class="stat">
                        <span class="stat-value" id="repoCount">--</span>
                        <span class="stat-label">Repos</span>
                    </div>
                    <div class="stat">
                        <span class="stat-value" id="followerCount">--</span>
                        <span class="stat-label">Followers</span>
                    </div>
                    <div class="stat">
                        <span class="stat-value" id="starCount">--</span>
                        <span class="stat-label">Stars</span>
                    </div>
                </div>
                <div class="github-repos">
                    <h4>Featured Repositories</h4>
                    <div class="repo-list" id="repoList">
                        <!-- Populated by JavaScript -->
                    </div>
                </div>
                <a href="https://github.com/aj-ajay-ay" class="github-link" target="_blank">
                    View Profile <i class="fas fa-arrow-right"></i>
                </a>
            </div>
            <div class="testimonial-card">
                <p class="testimonial-text">
                    Ajay's ability to break down complex infrastructure problems into elegant, maintainable solutions is exceptional. His work on our cloud migration reduced our deployment time by 80% while improving reliability.
                </p>
                <div class="testimonial-author">
                    <div class="author-avatar">JD</div>
                    <div class="author-info">
                        <h4>John Doe</h4>
                        <p>Engineering Manager, Tech Corp</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
```

**Step 3: Add GitHub API JavaScript**

Add to the script section:

```javascript
// Fetch GitHub Stats
async function fetchGitHubStats() {
    const username = 'aj-ajay-ay'; // Update with your GitHub username

    try {
        // Fetch user data
        const userResponse = await fetch(`https://api.github.com/users/${username}`);
        const userData = await userResponse.json();

        // Update stats
        document.getElementById('repoCount').textContent = userData.public_repos || '--';
        document.getElementById('followerCount').textContent = userData.followers || '--';

        // Fetch repos to calculate total stars
        const reposResponse = await fetch(`https://api.github.com/users/${username}/repos?per_page=100&sort=stars&direction=desc`);
        const repos = await reposResponse.json();

        // Calculate total stars
        const totalStars = repos.reduce((sum, repo) => sum + repo.stargazers_count, 0);
        document.getElementById('starCount').textContent = totalStars;

        // Display top 3 repos
        const repoList = document.getElementById('repoList');
        const topRepos = repos.slice(0, 3);

        repoList.innerHTML = topRepos.map(repo => `
            <div class="repo-item">
                <a href="${repo.html_url}" class="repo-name" target="_blank">${repo.name}</a>
                <span class="repo-stars">
                    <i class="fas fa-star"></i> ${repo.stargazers_count}
                </span>
            </div>
        `).join('');

    } catch (error) {
        console.log('GitHub API error:', error);
        // Graceful fallback - hide stats on error
    }
}

// Call on page load
fetchGitHubStats();
```

**Step 4: Verify in browser**

- GitHub card should display stats (repos, followers, stars)
- Top 3 repos should appear with star counts
- Testimonial card should have decorative quote mark
- Both cards should hover with lift effect
- Works in both themes

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add social proof section with GitHub stats and testimonial"
```

---

## Task 6: Skills Section — Flexible Card Grid

**Files:**
- Modify: `index.html` (style section + skills HTML)

**Step 1: Add skills styles**

```css
.skills-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
}

.skill-card {
    background-color: var(--bg-secondary);
    border: 1px solid var(--border);
    border-radius: var(--card-radius);
    padding: 2rem;
    transition: all var(--transition-medium);
}

.section:nth-child(even) .skill-card {
    background-color: var(--bg-primary);
}

.skill-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 30px var(--shadow-hover);
    border-color: var(--accent);
}

.skill-card h3 {
    font-family: var(--font-body);
    font-size: 1.1rem;
    font-weight: 600;
    color: var(--text-primary);
    margin-bottom: 0.75rem;
    display: flex;
    align-items: center;
    gap: 0.5rem;
}

.skill-card h3::before {
    content: '';
    width: 8px;
    height: 8px;
    background-color: var(--accent);
    border-radius: 50%;
    flex-shrink: 0;
}

.skill-card p {
    font-size: 0.95rem;
    color: var(--text-secondary);
    line-height: 1.6;
}
```

**Step 2: Update Skills section HTML**

```html
<section id="skills" class="section fade-in">
    <div class="container">
        <div class="section-header">
            <h2 class="section-title">Skills</h2>
        </div>
        <div class="skills-grid">
            <div class="skill-card">
                <h3>Cloud Architecture</h3>
                <p>Designing and implementing scalable, secure cloud solutions on AWS. Infrastructure as code with Terraform.</p>
            </div>
            <div class="skill-card">
                <h3>DevOps & Automation</h3>
                <p>CI/CD pipelines, GitOps practices, and infrastructure automation. Making deployments boring and reliable.</p>
            </div>
            <div class="skill-card">
                <h3>Backend Engineering</h3>
                <p>Python, FastAPI, Django, Flask. Building robust APIs and microservices that scale.</p>
            </div>
            <div class="skill-card">
                <h3>Containerization</h3>
                <p>Docker and Kubernetes for consistent deployments across environments. Container orchestration at scale.</p>
            </div>
            <div class="skill-card">
                <h3>Data Systems</h3>
                <p>PostgreSQL, MongoDB, Elasticsearch. Designing data pipelines with Kafka for real-time processing.</p>
            </div>
            <div class="skill-card">
                <h3>System Design</h3>
                <p>Distributed systems, high availability, fault tolerance. Breaking down complexity into simple, composable parts.</p>
            </div>
        </div>
    </div>
</section>
```

**Step 3: Verify in browser**

- Skills should display in responsive grid
- Add/remove cards to test flexibility
- Hover should show subtle lift and accent border
- Works in both themes

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add flexible skills grid with minimal card design"
```

---

## Task 7: Projects Section — Media-Ready Cards with Impact Metrics

**Files:**
- Modify: `index.html` (style section + projects HTML)

**Step 1: Add projects styles**

```css
.projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
}

.project-card {
    background-color: var(--bg-secondary);
    border: 1px solid var(--border);
    border-radius: var(--card-radius);
    overflow: hidden;
    transition: all var(--transition-medium);
}

.section:nth-child(even) .project-card {
    background-color: var(--bg-primary);
}

.project-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 12px 40px var(--shadow-hover);
}

.project-card:hover .project-media img,
.project-card:hover .project-media video {
    transform: scale(1.03);
}

.project-media {
    aspect-ratio: 16 / 10;
    overflow: hidden;
    background-color: var(--bg-tertiary);
}

.project-media img,
.project-media video {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform var(--transition-medium);
}

.project-content {
    padding: 1.5rem;
}

.project-content h3 {
    font-family: var(--font-body);
    font-size: 1.1rem;
    font-weight: 600;
    color: var(--text-primary);
    margin-bottom: 0.5rem;
}

.project-content p {
    font-size: 0.9rem;
    color: var(--text-secondary);
    margin-bottom: 1rem;
    line-height: 1.5;
}

.project-impact {
    font-family: var(--font-mono);
    font-size: 0.8rem;
    color: var(--accent);
    font-weight: 500;
    padding: 0.5rem 0;
    border-top: 1px dashed var(--border);
    margin-top: 0.5rem;
}

.project-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
}

.project-tag {
    font-family: var(--font-mono);
    font-size: 0.75rem;
    color: var(--accent);
    background-color: var(--accent-subtle);
    padding: 0.25rem 0.6rem;
    border-radius: 3px;
}

.project-link {
    display: inline-flex;
    align-items: center;
    gap: 0.4rem;
    font-size: 0.85rem;
    font-weight: 500;
    color: var(--accent);
    text-decoration: none;
    margin-top: 1rem;
    opacity: 0;
    transform: translateX(-8px);
    transition: all var(--transition-fast);
}

.project-card:hover .project-link {
    opacity: 1;
    transform: translateX(0);
}

.project-link:hover {
    color: var(--accent-hover);
}
```

**Step 2: Update Projects section HTML**

Each project card now includes an **impact metric** to quantify business value:

```html
<section id="projects" class="section fade-in">
    <div class="container">
        <div class="section-header">
            <h2 class="section-title">Projects</h2>
        </div>
        <div class="projects-grid">
            <div class="project-card">
                <div class="project-media">
                    <img src="assets/images/project1.jpg" alt="Cloud Infrastructure Automation">
                </div>
                <div class="project-content">
                    <h3>Cloud Infrastructure Automation</h3>
                    <p>Automated multi-region AWS infrastructure deployment with Terraform modules and GitOps workflows.</p>
                    <p class="project-impact">Reduced provisioning time from 2 days to 15 minutes</p>
                    <div class="project-tags">
                        <span class="project-tag">Terraform</span>
                        <span class="project-tag">AWS</span>
                        <span class="project-tag">GitOps</span>
                    </div>
                    <a href="#" class="project-link">View Project <i class="fas fa-arrow-right"></i></a>
                </div>
            </div>
            <div class="project-card">
                <div class="project-media">
                    <img src="assets/images/project2.jpg" alt="Microservices Platform">
                </div>
                <div class="project-content">
                    <h3>Microservices Platform</h3>
                    <p>Scalable microservices architecture with service mesh, distributed tracing, and automated scaling.</p>
                    <p class="project-impact">Handles 50K+ requests/second with 99.9% uptime</p>
                    <div class="project-tags">
                        <span class="project-tag">Kubernetes</span>
                        <span class="project-tag">Docker</span>
                        <span class="project-tag">Istio</span>
                    </div>
                    <a href="#" class="project-link">View Project <i class="fas fa-arrow-right"></i></a>
                </div>
            </div>
            <div class="project-card">
                <div class="project-media">
                    <img src="assets/images/project3.jpg" alt="Real-time Data Pipeline">
                </div>
                <div class="project-content">
                    <h3>Real-time Data Pipeline</h3>
                    <p>Event-driven data pipeline processing millions of events with exactly-once semantics.</p>
                    <p class="project-impact">Processing 2M+ events/day with sub-second latency</p>
                    <div class="project-tags">
                        <span class="project-tag">Kafka</span>
                        <span class="project-tag">Elasticsearch</span>
                        <span class="project-tag">Python</span>
                    </div>
                    <a href="#" class="project-link">View Project <i class="fas fa-arrow-right"></i></a>
                </div>
            </div>
        </div>
    </div>
</section>
```

**Step 3: Verify in browser**

- Project cards should display with images
- Hover should show subtle scale and "View Project" link
- Tags should use accent color
- Grid should reflow on resize

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add media-ready project cards with tech tags"
```

---

## Task 8: Certifications Section

**Files:**
- Modify: `index.html` (style section + certifications HTML)

**Step 1: Add certifications styles**

```css
.certifications-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
}

.certification-card {
    background-color: var(--bg-secondary);
    border: 1px solid var(--border);
    border-radius: var(--card-radius);
    padding: 2rem;
    text-align: center;
    transition: all var(--transition-medium);
}

.section:nth-child(even) .certification-card {
    background-color: var(--bg-primary);
}

.certification-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 30px var(--shadow-hover);
}

.certification-card img {
    max-width: 120px;
    height: auto;
    margin-bottom: 1.25rem;
    filter: grayscale(20%);
    transition: filter var(--transition-medium);
}

.certification-card:hover img {
    filter: grayscale(0%);
}

.certification-card h3 {
    font-family: var(--font-body);
    font-size: 1rem;
    font-weight: 600;
    color: var(--text-primary);
    margin-bottom: 0.5rem;
}

.certification-card p {
    font-size: 0.85rem;
    color: var(--text-muted);
}
```

**Step 2: Update Certifications HTML**

```html
<section id="certifications" class="section fade-in">
    <div class="container">
        <div class="section-header">
            <h2 class="section-title">Certifications</h2>
        </div>
        <div class="certifications-grid">
            <div class="certification-card">
                <img src="assets/images/certifications/aws-solutions-architect.png" alt="AWS Solutions Architect">
                <h3>AWS Solutions Architect</h3>
                <p>Professional certification for cloud architecture</p>
            </div>
        </div>
    </div>
</section>
```

**Step 3: Verify in browser**

- Certification card should display cleanly
- Image should be slightly desaturated, full color on hover

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add minimal certifications section"
```

---

## Task 9: Contact Section & Form

**Files:**
- Modify: `index.html` (style section + contact HTML)

**Step 1: Add contact styles**

```css
.contact-content {
    display: flex;
    flex-direction: column;
    gap: 2rem;
}

.contact-text {
    max-width: 50ch;
}

.contact-text p {
    color: var(--text-secondary);
}

.contact-links {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
}

.contact-link {
    display: inline-flex;
    align-items: center;
    gap: 0.6rem;
    font-size: 0.95rem;
    color: var(--text-secondary);
    text-decoration: none;
    padding: 0.75rem 1.25rem;
    border: 1px solid var(--border);
    border-radius: 4px;
    transition: all var(--transition-fast);
}

.contact-link:hover {
    color: var(--accent);
    border-color: var(--accent);
    background-color: var(--accent-subtle);
}

.contact-link i {
    font-size: 1.1rem;
}

/* Slide-in Form */
.overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(4px);
    z-index: 1000;
    opacity: 0;
    pointer-events: none;
    transition: opacity var(--transition-medium);
}

.overlay.active {
    opacity: 1;
    pointer-events: auto;
}

.contact-form-panel {
    position: fixed;
    top: 0;
    right: 0;
    width: min(500px, 90vw);
    height: 100vh;
    background-color: var(--bg-primary);
    padding: 3rem 2rem;
    z-index: 1001;
    transform: translateX(100%);
    transition: transform var(--transition-medium);
    overflow-y: auto;
    display: flex;
    flex-direction: column;
}

.contact-form-panel.active {
    transform: translateX(0);
}

.contact-form-panel h3 {
    font-family: var(--font-display);
    font-size: 1.75rem;
    color: var(--text-primary);
    margin-bottom: 0.5rem;
}

.contact-form-panel .subtitle {
    color: var(--text-muted);
    margin-bottom: 2rem;
}

.form-group {
    margin-bottom: 1.5rem;
}

.form-group label {
    display: block;
    font-size: 0.85rem;
    font-weight: 500;
    color: var(--text-secondary);
    margin-bottom: 0.5rem;
}

.form-group input,
.form-group textarea {
    width: 100%;
    padding: 0.875rem 1rem;
    font-family: var(--font-body);
    font-size: 0.95rem;
    color: var(--text-primary);
    background-color: var(--bg-secondary);
    border: 1px solid var(--border);
    border-radius: 4px;
    transition: all var(--transition-fast);
}

.form-group input:focus,
.form-group textarea:focus {
    outline: none;
    border-color: var(--accent);
    box-shadow: 0 0 0 3px var(--accent-subtle);
}

.form-group textarea {
    min-height: 140px;
    resize: vertical;
}

.form-submit {
    width: 100%;
    padding: 1rem;
    font-family: var(--font-body);
    font-size: 0.95rem;
    font-weight: 500;
    color: var(--bg-primary);
    background-color: var(--text-primary);
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: all var(--transition-fast);
}

.form-submit:hover {
    background-color: var(--accent);
}

.close-form {
    position: absolute;
    top: 1.5rem;
    right: 1.5rem;
    background: none;
    border: none;
    color: var(--text-muted);
    font-size: 1.5rem;
    cursor: pointer;
    transition: color var(--transition-fast);
}

.close-form:hover {
    color: var(--text-primary);
}
```

**Step 2: Update Contact HTML**

```html
<section id="contact" class="section fade-in">
    <div class="container">
        <div class="section-header">
            <h2 class="section-title">Contact</h2>
        </div>
        <div class="contact-content">
            <div class="contact-text">
                <p>Interested in working together or just want to chat about technology? I'd love to hear from you.</p>
            </div>
            <div class="contact-links">
                <a href="https://linkedin.com/in/ajay-renganathan" class="contact-link" target="_blank">
                    <i class="fab fa-linkedin"></i> LinkedIn
                </a>
                <a href="mailto:ajay.renganathan@gmail.com" class="contact-link">
                    <i class="fas fa-envelope"></i> Email
                </a>
                <a href="#" class="contact-link" onclick="openContactForm(); return false;">
                    <i class="fas fa-paper-plane"></i> Send Message
                </a>
            </div>
        </div>
    </div>
</section>

<div class="overlay" id="overlay" onclick="closeContactForm()"></div>
<div class="contact-form-panel" id="contactFormPanel">
    <button class="close-form" onclick="closeContactForm()">&times;</button>
    <h3>Send a Message</h3>
    <p class="subtitle">I'll get back to you as soon as I can.</p>
    <form id="messageForm">
        <div class="form-group">
            <label for="email">Your Email</label>
            <input type="email" id="email" name="email" required placeholder="you@example.com">
        </div>
        <div class="form-group">
            <label for="message">Message</label>
            <textarea id="message" name="message" required placeholder="What's on your mind?"></textarea>
        </div>
        <button type="submit" class="form-submit">Send Message</button>
    </form>
</div>
```

**Step 3: Add contact form JavaScript**

```javascript
// Contact Form
function openContactForm() {
    document.getElementById('overlay').classList.add('active');
    document.getElementById('contactFormPanel').classList.add('active');
    document.body.style.overflow = 'hidden';
}

function closeContactForm() {
    document.getElementById('overlay').classList.remove('active');
    document.getElementById('contactFormPanel').classList.remove('active');
    document.body.style.overflow = '';
}

document.getElementById('messageForm').addEventListener('submit', (e) => {
    e.preventDefault();
    const email = document.getElementById('email').value;
    const message = document.getElementById('message').value;

    // Here you would send to your backend
    console.log('Message:', { email, message });

    alert('Thank you for your message! I\'ll get back to you soon.');
    e.target.reset();
    closeContactForm();
});

// Close form on Escape key
document.addEventListener('keydown', (e) => {
    if (e.key === 'Escape') closeContactForm();
});
```

**Step 4: Verify in browser**

- Contact links should be clean and inline
- "Send Message" opens slide-in panel
- Form matches theme
- Escape key and overlay click close the form

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: redesign contact section with slide-in form panel"
```

---

## Task 10: Footer & Final Cleanup

**Files:**
- Modify: `index.html`

**Step 1: Add minimal footer styles**

```css
footer {
    padding: 3rem 2rem;
    text-align: center;
    border-top: 1px solid var(--border);
}

footer p {
    font-size: 0.85rem;
    color: var(--text-muted);
}

footer a {
    color: var(--accent);
    text-decoration: none;
}

footer a:hover {
    text-decoration: underline;
}
```

**Step 2: Add footer HTML**

After the contact section, before closing body:

```html
<footer>
    <p>Built with curiosity by Ajay Renganathan</p>
</footer>
```

**Step 3: Remove old/unused CSS**

Delete all previously existing styles that are no longer used:
- Old gradient backgrounds
- Old scroll indicator styles
- Old floating logos collision styles
- Duplicate style rules
- Remove `scroll-indicator` element if still present

**Step 4: Remove old/unused JavaScript**

Delete:
- Old `toggleContactForm` function
- Old collision detection code
- Old scroll indicator code
- Old ripple animation code

**Step 5: Verify complete site**

- Test both themes thoroughly
- Test all navigation links
- Test mobile responsive layout
- Test contact form
- Verify no console errors

**Step 6: Final commit**

```bash
git add index.html
git commit -m "feat: add footer and clean up unused styles and scripts"
```

---

## Task 11: Section Order & Polish

**Files:**
- Modify: `index.html`

**Step 1: Reorder sections in HTML**

Ensure sections appear in this order:
1. Hero
2. About
3. Social Proof (GitHub + Testimonial)
4. Skills
5. Projects
6. Certifications
7. Contact
8. Footer

**Step 2: Add subtle section transitions for theme switch**

Update section styles to include:

```css
.section {
    transition: background-color var(--transition-medium);
}
```

**Step 3: Test complete flow**

- Navigate through all sections
- Toggle themes at various scroll positions
- Test on mobile viewport
- Verify smooth scrolling to all sections
- Verify grain texture is subtle in both themes
- Verify theme toggle animation is smooth

**Step 4: Commit**

```bash
git add index.html
git commit -m "polish: finalize section order and theme transitions"
```

---

## Summary

This plan transforms the portfolio from a visually busy gradient site into a refined, dual-theme experience that reflects your philosophy of simplicity and curiosity. Key outcomes:

- **Two cohesive themes** with easy color customization via CSS custom properties
- **Typography-forward hero** with tightened tagline: "Software architect. Perpetually curious."
- **Stronger CTA** — "Let's Build Something" matches curious builder persona
- **Subtle tech backdrop** — logos at 5-8% opacity, slow drift, texture not entertainment
- **Animated theme toggle** — smooth icon rotation/morph transition
- **Distinctive pull quote** — centered with decorative quotation mark and accent underline
- **Social proof section** — Live GitHub stats (repos, followers, stars) + testimonial card
- **Subtle grain texture** — adds tactile warmth without distraction
- **Flexible grids** for skills and projects that scale
- **Media-ready project cards** with **impact metrics** (e.g., "Reduced provisioning time by 80%")
- **Minimal, purposeful animations** — fade-ins, subtle hovers
- **Clean code organization** — all colors swappable from `:root`

Total: 11 tasks, ~60-75 minutes implementation time.
