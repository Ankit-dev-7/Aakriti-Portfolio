# Design Document: Aakriti Aryal Portfolio

## Overview

This document describes the technical design for Aakriti Aryal's personal portfolio website — a single-file static page (`index.html`) targeting TikTok content creators and brand partners. The site presents a premium creator brand aesthetic: deep charcoal backgrounds, soft rose gold / muted lavender accents, generous whitespace, and refined micro-interactions. All CSS and JavaScript are inlined in the HTML file; no build tools, bundlers, or backend are required.

**Design philosophy**: Think Rhode by Hailey Bieber — luxury, editorial, restrained. The palette and typography do the heavy lifting; animations are subtle and purposeful rather than flashy.

**Key constraints**:
- Single `index.html` file, works via `file://` protocol
- Vanilla JavaScript only (no React, Vue, jQuery, etc.)
- Google Fonts (Poppins) and Font Awesome loaded via CDN
- Fully responsive, mobile-first

---

## Architecture

The entire application is a single HTML document with three logical layers:

```
index.html
├── <head>
│   ├── Meta tags (viewport, charset, SEO)
│   ├── Google Fonts link (Poppins 300/400/600/700)
│   ├── Font Awesome CDN link (icons)
│   └── <style> — all CSS (mobile-first, custom properties, animations)
├── <body>
│   ├── <header> — Nav_Bar (sticky, hamburger on mobile)
│   ├── <main>
│   │   ├── <section id="hero">    — Hero_Section
│   │   ├── <section id="videos"> — Videos_Section
│   │   ├── <section id="about">  — About_Section
│   │   ├── <section id="stats">  — Stats_Section
│   │   ├── <section id="gallery">— Gallery_Section
│   │   └── <section id="contact">— Contact_Section
│   ├── <footer>                  — Footer
│   └── <button id="back-to-top"> — Back_To_Top_Button
└── <script> — all JavaScript (scroll animations, counters, form, nav toggle)
```

### Data flow

Since there is no backend, all data is hardcoded in the HTML. The JavaScript layer handles:
1. **Scroll events** → show/hide back-to-top button, trigger Intersection Observer
2. **Intersection Observer** → add `.visible` class to sections (one-shot)
3. **Counter animation** → `requestAnimationFrame` loop per stat item
4. **Nav toggle** → toggle `.open` class on mobile nav
5. **Form submission** → `preventDefault`, validate, show inline message

```
User scrolls
    │
    ├─► scroll event listener
    │       └─► toggles #back-to-top visibility (threshold: 300px)
    │
    └─► IntersectionObserver
            └─► adds .visible to observed elements (once, then unobserves)
                    └─► CSS transition plays (opacity + translateY)
                    └─► if stat section: triggers counter animation
```

---

## Components and Interfaces

### 1. CSS Custom Properties (Design Tokens)

All colors, spacing, and typography values are defined as CSS custom properties on `:root` for consistency and easy theming.

```css
:root {
  /* Backgrounds */
  --bg-primary:    #0a0a0f;
  --bg-secondary:  #111118;
  --bg-card:       #16161f;
  --bg-card-hover: #1c1c28;

  /* Accent palette — used sparingly */
  --accent-rose:    #e8a598;
  --accent-blush:   #f2c4ce;
  --accent-lavender:#a78bfa;
  --accent-primary: var(--accent-rose); /* default CTA accent */

  /* Text */
  --text-primary:   #f0ece8;
  --text-secondary: #9e9aaa;
  --text-muted:     #5c5870;

  /* Gradients */
  --gradient-hero:  linear-gradient(135deg, #0a0a0f 0%, #1a0f2e 50%, #0d0d1a 100%);
  --gradient-stats: linear-gradient(135deg, #111118 0%, #1a0f2e 100%);
  --gradient-card:  linear-gradient(145deg, #16161f, #1c1c28);

  /* Glow / shadow */
  --glow-accent:    0 0 20px rgba(232, 165, 152, 0.25);
  --shadow-card:    0 4px 24px rgba(0, 0, 0, 0.4);
  --shadow-hover:   0 8px 40px rgba(232, 165, 152, 0.2);

  /* Typography */
  --font-family:    'Poppins', sans-serif;
  --font-light:     300;
  --font-regular:   400;
  --font-semibold:  600;
  --font-bold:      700;

  /* Spacing scale */
  --space-xs:  0.5rem;
  --space-sm:  1rem;
  --space-md:  2rem;
  --space-lg:  4rem;
  --space-xl:  6rem;

  /* Transitions */
  --transition-fast:   0.3s ease;
  --transition-medium: 0.5s ease-out;
  --transition-slow:   0.8s ease-out;

  /* Border radius */
  --radius-sm:  8px;
  --radius-md:  16px;
  --radius-lg:  24px;
  --radius-pill:9999px;
}
```

### 2. Nav_Bar Component

**HTML structure**:
```html
<header>
  <nav class="navbar">
    <a href="#hero" class="nav-brand">Aakriti Aryal</a>
    <button class="hamburger" aria-label="Toggle navigation" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>
    <ul class="nav-links">
      <li><a href="#videos">Videos</a></li>
      <li><a href="#about">About</a></li>
      <li><a href="#stats">Stats</a></li>
      <li><a href="#gallery">Gallery</a></li>
      <li><a href="#contact" class="nav-cta">Contact</a></li>
    </ul>
  </nav>
</header>
```

**Behavior**:
- `position: fixed`, `top: 0`, full width, `z-index: 1000`
- Background: `rgba(10, 10, 15, 0.85)` with `backdrop-filter: blur(12px)` for frosted glass effect
- Brand name styled in accent color, font-weight 600
- On mobile (`< 768px`): hamburger button visible, nav-links hidden by default; JS toggles `.open` class
- Hamburger JS updates `aria-expanded` attribute for accessibility

### 3. Hero_Section Component

**HTML structure**:
```html
<section id="hero" class="hero">
  <div class="hero-bg"></div>  <!-- gradient + subtle animated orbs -->
  <div class="hero-content">
    <div class="hero-image-wrapper">
      <div class="hero-image-placeholder" aria-label="Aakriti Aryal profile photo">
        <!-- placeholder or <img> -->
      </div>
    </div>
    <div class="hero-text">
      <h1 class="hero-name">Aakriti Aryal</h1>
      <p class="hero-tagline">Content Creator &amp; Digital Personality</p>
      <div class="hero-social"><!-- social icon links --></div>
      <div class="hero-actions">
        <a href="https://www.tiktok.com/@aakriti__aryal" target="_blank" rel="noopener noreferrer" class="btn btn-primary">
          Follow on TikTok
        </a>
        <a href="#contact" class="btn btn-outline">Contact Me</a>
      </div>
    </div>
  </div>
</section>
```

**Visual design**:
- Full `100vh`, gradient background (`--gradient-hero`)
- Two subtle radial gradient "orbs" positioned absolutely, animated with a slow `pulse` keyframe (opacity 0.3 → 0.6, scale 1 → 1.1, 6s infinite alternate) — elegant, not distracting
- Profile image: circular, 200px diameter, bordered with 2px `--accent-rose` ring, soft glow shadow
- Placeholder: gradient fill (`--gradient-card`) with initials "AA" centered
- `.btn-primary`: filled with `--accent-rose`, dark text, pill shape, hover lifts with `--shadow-hover`
- `.btn-outline`: transparent with `--accent-rose` border, hover fills

### 4. Videos_Section Component

**HTML structure** (repeated for each card):
```html
<section id="videos" class="section videos-section">
  <h2 class="section-title">Featured Videos</h2>
  <div class="videos-grid">
    <div class="video-card">
      <div class="video-placeholder">
        <div class="play-icon"><i class="fas fa-play"></i></div>
        <div class="video-label">@aakriti__aryal</div>
      </div>
    </div>
    <!-- repeat × 6 -->
  </div>
</section>
```

**Visual design**:
- Grid: `grid-template-columns: repeat(auto-fill, minmax(200px, 1fr))` — 3+ columns on desktop
- Card aspect ratio: `aspect-ratio: 9/16` (TikTok vertical format)
- Card background: `--gradient-card`, border-radius `--radius-md`
- Hover: `transform: scale(1.05)`, `box-shadow: var(--shadow-hover)`, transition `--transition-fast`
- Play icon: centered, accent color, 2rem size
- If real TikTok iframes are embedded: wrapped in `aspect-ratio: 9/16` container

### 5. About_Section Component

**HTML structure**:
```html
<section id="about" class="section about-section">
  <h2 class="section-title">About Me</h2>
  <div class="about-content">
    <div class="about-image">
      <div class="about-image-placeholder" aria-label="Aakriti Aryal photo"></div>
    </div>
    <div class="about-text">
      <p>Lifestyle and entertainment content creator based in Nepal, known for engaging and relatable short-form videos.</p>
      <!-- additional bio paragraphs -->
    </div>
  </div>
</section>
```

**Layout**: Two-column on desktop (`grid-template-columns: 1fr 1fr`), single column on mobile.

### 6. Stats_Section Component

**HTML structure**:
```html
<section id="stats" class="section stats-section">
  <div class="stats-grid">
    <div class="stat-item" data-target="500000" data-suffix="K+" data-divisor="1000">
      <i class="fas fa-users stat-icon"></i>
      <span class="stat-number" data-count="500">0</span><span class="stat-unit">K+</span>
      <p class="stat-label">Followers</p>
    </div>
    <div class="stat-item" data-target="2000000" data-suffix="M+" data-divisor="1000000">
      <i class="fas fa-heart stat-icon"></i>
      <span class="stat-number" data-count="2">0</span><span class="stat-unit">M+</span>
      <p class="stat-label">Total Likes</p>
    </div>
    <div class="stat-item" data-target="8" data-suffix="%" data-divisor="1">
      <i class="fas fa-chart-line stat-icon"></i>
      <span class="stat-number" data-count="8">0</span><span class="stat-unit">%</span>
      <p class="stat-label">Engagement Rate</p>
    </div>
  </div>
</section>
```

**Counter animation interface** (JavaScript):
```javascript
/**
 * Animates a numeric counter from 0 to target over duration ms.
 * Uses requestAnimationFrame for smooth 60fps animation.
 *
 * @param {HTMLElement} el      - The element whose textContent is updated
 * @param {number}      target  - The final numeric value to count to
 * @param {number}      duration- Animation duration in milliseconds (1500–2500)
 */
function animateCounter(el, target, duration) { ... }
```

**Visual design**:
- Background: `--gradient-stats` (deep purple-to-black)
- Stat items: centered, icon in accent color above number, large bold number, muted label
- Icon: Font Awesome, `--accent-rose` color, 2rem

### 7. Gallery_Section Component

**HTML structure**:
```html
<section id="gallery" class="section gallery-section">
  <h2 class="section-title">Gallery</h2>
  <div class="gallery-grid">
    <div class="gallery-item">
      <div class="gallery-placeholder" aria-label="Gallery photo 1"></div>
      <div class="gallery-overlay"></div>
    </div>
    <!-- repeat × 9 -->
  </div>
</section>
```

**Visual design**:
- Grid: `repeat(auto-fill, minmax(250px, 1fr))`, 3+ columns on desktop
- Items: `aspect-ratio: 1/1` (square), `overflow: hidden`, `border-radius: --radius-md`
- Placeholder: unique gradient per item (cycling through accent-adjacent hues)
- Hover: inner image/placeholder `transform: scale(1.1)`, overlay fades in (semi-transparent dark + accent tint)
- Transition: `--transition-fast` on transform, `--transition-medium` on overlay

### 8. Contact_Section Component

**HTML structure**:
```html
<section id="contact" class="section contact-section">
  <h2 class="section-title">Work With Me</h2>
  <p class="contact-intro">Available for brand collaborations and promotions</p>
  <form id="contact-form" novalidate>
    <div class="form-group">
      <label for="name">Name</label>
      <input type="text" id="name" name="name" placeholder="Your name" required>
      <span class="field-error" id="name-error"></span>
    </div>
    <div class="form-group">
      <label for="email">Email</label>
      <input type="email" id="email" name="email" placeholder="your@email.com" required>
      <span class="field-error" id="email-error"></span>
    </div>
    <div class="form-group">
      <label for="message">Message</label>
      <textarea id="message" name="message" rows="5" placeholder="Tell me about your project..." required></textarea>
      <span class="field-error" id="message-error"></span>
    </div>
    <button type="submit" class="btn btn-primary">Send Message</button>
    <div id="form-success" class="form-success" hidden>
      Thanks! Your message has been sent.
    </div>
  </form>
</section>
```

**Form validation interface** (JavaScript):
```javascript
/**
 * Validates the contact form fields.
 * Returns an object mapping field names to error messages (empty string = valid).
 *
 * @param  {{ name: string, email: string, message: string }} fields
 * @returns {{ name: string, email: string, message: string }}
 */
function validateForm(fields) { ... }
```

**Behavior**:
- `novalidate` on form to suppress native browser UI; JS handles all validation
- On submit: call `validateForm`, display per-field errors if any, show success message if all valid
- Success message shown inline (no page reload), form fields cleared

### 9. Footer Component

```html
<footer>
  <div class="footer-content">
    <div class="footer-social"><!-- social icon links --></div>
    <p class="footer-copy">© <span id="footer-year"></span> Aakriti Aryal. All rights reserved.</p>
  </div>
</footer>
```

Year is set dynamically: `document.getElementById('footer-year').textContent = new Date().getFullYear();`

### 10. Back_To_Top_Button Component

```html
<button id="back-to-top" aria-label="Back to top" class="back-to-top">
  <i class="fas fa-chevron-up"></i>
</button>
```

**Behavior**: Scroll listener checks `window.scrollY > 300`; toggles `.visible` class which transitions opacity 0→1 and translateY 20px→0.

---

## Data Models

Since this is a static site with no backend, "data" is hardcoded HTML content. The following describes the logical data structures used by JavaScript.

### StatItem

```typescript
interface StatItem {
  target: number;      // Final numeric value (e.g., 500 for "500K+")
  suffix: string;      // Display suffix (e.g., "K+", "M+", "%")
  label: string;       // Human-readable label (e.g., "Followers")
  icon: string;        // Font Awesome class (e.g., "fas fa-users")
  duration: number;    // Animation duration in ms (1500–2500)
}
```

### FormFields

```typescript
interface FormFields {
  name: string;
  email: string;
  message: string;
}

interface ValidationResult {
  name: string;    // Error message or empty string
  email: string;
  message: string;
  isValid: boolean;
}
```

### ScrollAnimationState

```typescript
interface AnimatedElement {
  el: HTMLElement;
  hasAnimated: boolean;  // true once .visible has been added (prevents replay)
}
```

---

## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system — essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*

### Property 1: Counter animation reaches target

*For any* stat item with a positive integer target value and a duration between 1500ms and 2500ms, the counter animation SHALL start at 0 and reach exactly the target value by the time the animation completes.

**Validates: Requirements 8.3**

---

### Property 2: Form validation rejects any combination of empty fields

*For any* combination of form field values where at least one field (name, email, or message) is empty or whitespace-only, the `validateForm` function SHALL return a non-empty error message for that field and `isValid: false`, and the success message SHALL NOT be displayed.

**Validates: Requirements 10.4, 10.6**

---

### Property 3: Scroll animation triggers exactly once per element

*For any* section element observed by the Intersection Observer, the `.visible` class SHALL be added exactly once when the element first enters the viewport, and SHALL NOT be removed or re-added on subsequent scroll events (no replay).

**Validates: Requirements 14.2, 14.3**

---

### Property 4: All images have non-empty alt attributes

*For any* `<img>` element present in the document, the element SHALL have an `alt` attribute whose value is a non-empty string.

**Validates: Requirements 15.2**

---

### Property 5: Text contrast ratio meets WCAG AA

*For any* text element in the document, the computed contrast ratio between its foreground color and its effective background color SHALL be at least 4.5:1 for normal-sized text, in accordance with WCAG 2.1 AA guidelines.

**Validates: Requirements 15.5**

---

## Error Handling

### Form Submission Errors

| Scenario | Handling |
|---|---|
| Empty name field | Show "Name is required." below the input |
| Empty email field | Show "Email is required." below the input |
| Invalid email format | Show "Please enter a valid email address." |
| Empty message field | Show "Message is required." below the textarea |
| All fields valid | Hide all error messages, show success banner, clear form |

Error messages are displayed in `.field-error` spans adjacent to each input. They are shown/hidden by toggling `display: block` / `display: none` (or `hidden` attribute). ARIA: inputs get `aria-describedby` pointing to their error span; error spans get `role="alert"` so screen readers announce them.

### Media Loading Errors

All `<img>` elements include an `onerror` handler that swaps the broken image for the styled placeholder:

```javascript
img.onerror = function() {
  this.style.display = 'none';
  this.nextElementSibling.style.display = 'flex'; // show placeholder
};
```

### CDN Failure (Google Fonts / Font Awesome)

CSS includes a system font fallback stack:
```css
font-family: 'Poppins', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
```

If Font Awesome fails to load, icon elements degrade gracefully — they render as empty inline elements (no broken images, no layout shift) because Font Awesome icons are pseudo-elements.

### Intersection Observer Unavailability

A feature-detection guard ensures the scroll animation degrades gracefully on browsers without Intersection Observer support:

```javascript
if ('IntersectionObserver' in window) {
  // set up observer
} else {
  // immediately make all sections visible
  document.querySelectorAll('.animate-on-scroll').forEach(el => el.classList.add('visible'));
}
```

### Counter Animation Edge Cases

- Target value of 0: animation skips immediately, displays "0"
- Non-integer targets (e.g., 8% engagement): animation rounds to nearest integer at each frame
- `requestAnimationFrame` unavailable: falls back to `setTimeout` with 16ms interval

---

## Testing Strategy

### Overview

This feature is a static HTML/CSS/JS page. The primary testing concerns are:

1. **JavaScript logic correctness** — counter animation, form validation, scroll animation state
2. **Accessibility compliance** — alt attributes, contrast ratios, keyboard navigation
3. **Responsive layout** — breakpoint behavior at 768px and 1024px
4. **Visual regression** — design tokens applied correctly

Property-based testing (PBT) is applicable for the pure JavaScript functions (counter animation, form validation, scroll animation state machine). The chosen PBT library is **fast-check** (JavaScript), which can be loaded via CDN or used in a Node.js test environment.

### Unit Tests (Example-Based)

These cover specific scenarios and integration points:

| Test | What it verifies |
|---|---|
| Nav hamburger toggle | Clicking hamburger adds/removes `.open` class on mobile |
| Nav smooth scroll | Clicking a nav link calls `scrollIntoView` on the target section |
| Back-to-top visibility at 0px | Button has `display: none` / opacity 0 at scroll position 0 |
| Back-to-top visibility at 301px | Button becomes visible after scrolling past 300px |
| Back-to-top click | Clicking button calls `window.scrollTo({ top: 0, behavior: 'smooth' })` |
| Hero TikTok button | Button `href` is `https://www.tiktok.com/@aakriti__aryal`, `target="_blank"` |
| Hero Contact button | Clicking scrolls to `#contact` section |
| Form success flow | All fields filled → success message shown, form cleared |
| Form email format | `user@` (no domain) → email error shown |
| Footer year | `#footer-year` textContent equals current year |
| Placeholder rendering | Video and gallery placeholders render when no real media |

### Property-Based Tests

Each property test uses **fast-check** with a minimum of 100 iterations. Tests are tagged with the design property they validate.

**Feature: aakriti-aryal-portfolio, Property 1: Counter animation reaches target**
```javascript
// For any positive integer target and duration in [1500, 2500],
// animateCounter should end at exactly the target value.
fc.assert(fc.property(
  fc.integer({ min: 1, max: 10000000 }),
  fc.integer({ min: 1500, max: 2500 }),
  (target, duration) => {
    const el = document.createElement('span');
    animateCounter(el, target, duration);
    // advance all animation frames
    jest.runAllTimers();
    return parseInt(el.textContent) === target;
  }
), { numRuns: 100 });
```

**Feature: aakriti-aryal-portfolio, Property 2: Form validation rejects any combination of empty fields**
```javascript
// For any combination where at least one field is empty/whitespace,
// validateForm returns isValid: false and a non-empty error for that field.
fc.assert(fc.property(
  fc.record({
    name:    fc.oneof(fc.constant(''), fc.string().filter(s => s.trim() === ''), fc.string()),
    email:   fc.oneof(fc.constant(''), fc.string().filter(s => s.trim() === ''), fc.emailAddress()),
    message: fc.oneof(fc.constant(''), fc.string().filter(s => s.trim() === ''), fc.string()),
  }).filter(f => !f.name.trim() || !f.email.trim() || !f.message.trim()),
  (fields) => {
    const result = validateForm(fields);
    const hasError = (!fields.name.trim() && result.name !== '') ||
                     (!fields.email.trim() && result.email !== '') ||
                     (!fields.message.trim() && result.message !== '');
    return !result.isValid && hasError;
  }
), { numRuns: 200 });
```

**Feature: aakriti-aryal-portfolio, Property 3: Scroll animation triggers exactly once per element**
```javascript
// For any sequence of viewport enter/exit events on a section element,
// the .visible class is added exactly once and never removed.
fc.assert(fc.property(
  fc.array(fc.boolean(), { minLength: 1, maxLength: 20 }), // true = entering, false = leaving
  (intersectionSequence) => {
    const el = document.createElement('section');
    el.classList.add('animate-on-scroll');
    const observer = createScrollObserver(); // factory from our JS module
    intersectionSequence.forEach(isIntersecting => {
      simulateIntersection(observer, el, isIntersecting);
    });
    const addCount = countClassAdditions(el, 'visible');
    return addCount <= 1 && (intersectionSequence.some(Boolean) ? addCount === 1 : addCount === 0);
  }
), { numRuns: 100 });
```

**Feature: aakriti-aryal-portfolio, Property 4: All images have non-empty alt attributes**
```javascript
// For any img element in the rendered document,
// it must have a non-empty alt attribute.
const images = document.querySelectorAll('img');
fc.assert(fc.property(
  fc.integer({ min: 0, max: images.length - 1 }),
  (index) => {
    const img = images[index];
    return img.hasAttribute('alt') && img.getAttribute('alt').trim() !== '';
  }
), { numRuns: Math.max(images.length, 100) });
```

**Feature: aakriti-aryal-portfolio, Property 5: Text contrast ratio meets WCAG AA**
```javascript
// For any text element in the document,
// contrast ratio between text color and background >= 4.5:1.
const textElements = document.querySelectorAll('p, h1, h2, h3, h4, h5, h6, a, span, label, button');
fc.assert(fc.property(
  fc.integer({ min: 0, max: textElements.length - 1 }),
  (index) => {
    const el = textElements[index];
    const ratio = computeContrastRatio(el); // uses getComputedStyle
    return ratio >= 4.5;
  }
), { numRuns: Math.max(textElements.length, 100) });
```

### Accessibility Tests

- Keyboard navigation: Tab through all interactive elements in DOM order
- Screen reader: `aria-label` on icon-only buttons, `aria-expanded` on hamburger, `role="alert"` on error spans
- Focus styles: visible `:focus-visible` outline on all interactive elements (accent color ring)

### Responsive Layout Tests

Manual or automated (Playwright/Puppeteer) at three breakpoints:
- 375px (mobile): single-column grids, hamburger visible, nav links hidden
- 900px (tablet): two-column grids, full nav visible
- 1280px (desktop): three-column grids, full layout

### Visual Regression

Snapshot the rendered page at each breakpoint using a headless browser. Compare against approved baseline screenshots on any CSS change.
