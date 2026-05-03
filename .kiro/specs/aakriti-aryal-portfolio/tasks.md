# Implementation Plan: Aakriti Aryal Portfolio

## Overview

Build a single `index.html` file containing all CSS and JavaScript inline. The page is a premium dark-aesthetic influencer portfolio with a sticky frosted-glass navbar, animated hero, TikTok-style video grid, animated stat counters, square gallery grid, contact form with JS validation, scroll animations via Intersection Observer, and a back-to-top button. All interactivity is vanilla JavaScript; fonts and icons are loaded from CDN.

## Tasks

- [x] 1. Set up the HTML skeleton and design tokens
  - Create `index.html` with `<!DOCTYPE html>`, `<html lang="en">`, `<head>`, and `<body>` scaffolding
  - Add `<meta charset="UTF-8">`, `<meta name="viewport" content="width=device-width, initial-scale=1.0">`, and SEO meta tags
  - Add Google Fonts `<link>` for Poppins (weights 300, 400, 600, 700) and Font Awesome CDN `<link>`
  - Open a `<style>` block and define all CSS custom properties on `:root` (backgrounds, accent palette, text colors, gradients, glow/shadow tokens, typography, spacing scale, transitions, border radii) exactly as specified in the design document
  - Add CSS reset/base styles: `*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }`, `html { scroll-behavior: smooth; }`, `body` with `--bg-primary` background and `--font-family`
  - Add semantic structural elements: `<header>`, `<main>`, `<footer>`, `<button id="back-to-top">`, and an empty `<script>` block at the bottom of `<body>`
  - _Requirements: 1.1, 1.3, 2.5, 3.2, 15.3_

- [ ] 2. Implement the Nav_Bar component
  - [x] 2.1 Write the Nav_Bar HTML inside `<header>`
    - Add `<nav class="navbar">` with brand anchor `<a href="#hero" class="nav-brand">Aakriti Aryal</a>`
    - Add `<button class="hamburger" aria-label="Toggle navigation" aria-expanded="false">` with three `<span>` children
    - Add `<ul class="nav-links">` with `<li><a>` items for Videos, About, Stats, Gallery, and a `.nav-cta` Contact link
    - _Requirements: 4.2, 4.4_
  - [x] 2.2 Write Nav_Bar CSS
    - Style `.navbar` as `position: fixed; top: 0; width: 100%; z-index: 1000` with `background: rgba(10,10,15,0.85)` and `backdrop-filter: blur(12px)` for frosted glass
    - Style `.nav-brand` with `--accent-rose` color and `font-weight: var(--font-semibold)`
    - Style `.nav-links` as a flex row with gap; style `.nav-cta` as a pill button with accent fill
    - Add mobile styles (`@media (max-width: 767px)`): hide `.nav-links` by default, show `.hamburger`; when `.nav-links.open` is present, display as a column dropdown
    - Add `transition: var(--transition-fast)` to all nav links for hover color change
    - _Requirements: 4.1, 4.3, 4.5, 3.3_
  - [ ] 2.3 Write Nav_Bar JavaScript
    - Query `.hamburger` and `.nav-links`; add `click` listener on hamburger that toggles `.open` class on `.nav-links` and updates `aria-expanded` attribute
    - _Requirements: 4.5_

- [ ] 3. Implement the Hero_Section component
  - [x] 3.1 Write Hero_Section HTML inside `<section id="hero" class="hero">`
    - Add `<div class="hero-bg">` for gradient + orb layer
    - Add `<div class="hero-content">` containing: `.hero-image-wrapper` > `.hero-image-placeholder` (with `aria-label="Aakriti Aryal profile photo"` and initials "AA"), `.hero-text` > `<h1 class="hero-name">`, `<p class="hero-tagline">`, `.hero-social` with TikTok/Instagram/YouTube icon links, `.hero-actions` with `.btn.btn-primary` (TikTok link, `target="_blank" rel="noopener noreferrer"`) and `.btn.btn-outline` (href="#contact")
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.5, 5.6, 11.1, 11.2, 11.3_
  - [x] 3.2 Write Hero_Section CSS
    - Style `.hero` as `min-height: 100vh`, `display: flex`, `align-items: center`, `background: var(--gradient-hero)`, `position: relative`, `overflow: hidden`
    - Style `.hero-bg` with two absolutely-positioned radial gradient orbs; define `@keyframes pulse` (opacity 0.3→0.6, scale 1→1.1, 6s infinite alternate) and apply to each orb with offset delays
    - Style `.hero-image-placeholder` as a 200px circle with `border: 2px solid var(--accent-rose)`, `box-shadow: var(--glow-accent)`, gradient fill, centered "AA" initials in accent color
    - Style `.btn-primary` as pill shape with `background: var(--accent-rose)`, dark text, hover `box-shadow: var(--shadow-hover)` and slight `translateY(-2px)`
    - Style `.btn-outline` as transparent with `border: 1px solid var(--accent-rose)`, hover fills with accent
    - Style `.hero-social` icon links with accent color, hover scale
    - _Requirements: 5.1, 5.7, 3.1, 3.3, 3.4, 3.5_

- [ ] 4. Implement the Videos_Section component
  - [x] 4.1 Write Videos_Section HTML inside `<section id="videos" class="section videos-section">`
    - Add `<h2 class="section-title">Featured Videos</h2>`
    - Add `<div class="videos-grid">` containing 6 `.video-card` divs, each with `.video-placeholder` > `.play-icon` (`<i class="fas fa-play">`) and `.video-label` (`@aakriti__aryal`)
    - _Requirements: 6.1, 6.2, 6.4_
  - [x] 4.2 Write Videos_Section CSS
    - Style `.videos-grid` as `display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: var(--space-md)`
    - Style `.video-card` with `aspect-ratio: 9/16`, `background: var(--gradient-card)`, `border-radius: var(--radius-md)`, `overflow: hidden`, `position: relative`, `cursor: pointer`
    - Add hover rule: `transform: scale(1.05); box-shadow: var(--shadow-hover); transition: var(--transition-fast)`
    - Style `.play-icon` as centered absolutely-positioned element with accent color and `font-size: 2rem`
    - Style `.video-label` at bottom of card in muted text
    - _Requirements: 6.1, 6.3, 6.5, 3.5_

- [x] 5. Implement the About_Section component
  - Write About_Section HTML inside `<section id="about" class="section about-section">`
    - Add `<h2 class="section-title">About Me</h2>`
    - Add `<div class="about-content">` with `.about-image` > `.about-image-placeholder` (`aria-label="Aakriti Aryal photo"`) and `.about-text` > `<p>` with the required bio text plus additional paragraphs
  - Write About_Section CSS
    - Style `.about-content` as `display: grid; grid-template-columns: 1fr 1fr; gap: var(--space-lg)` on desktop; single column on mobile (`@media (max-width: 767px)`)
    - Style `.about-image-placeholder` as a rounded rectangle with gradient fill matching the card palette
    - _Requirements: 7.1, 7.2, 7.3, 7.4_

- [ ] 6. Implement the Stats_Section component
  - [x] 6.1 Write Stats_Section HTML inside `<section id="stats" class="section stats-section">`
    - Add `<div class="stats-grid">` with three `.stat-item` divs, each containing: Font Awesome icon (`<i>`), `<span class="stat-number">0</span>`, `<span class="stat-unit">`, `<p class="stat-label">`
    - Set `data-count` attributes to `500`, `2`, `8` and `data-suffix` to `K+`, `M+`, `%` respectively; set `data-duration` to values in the 1500–2500ms range
    - _Requirements: 8.1, 8.2, 8.5_
  - [x] 6.2 Write Stats_Section CSS
    - Style `.stats-section` with `background: var(--gradient-stats)`
    - Style `.stats-grid` as a flex row (or 3-column grid) with centered items
    - Style `.stat-number` as large bold text in `--text-primary`; style `.stat-unit` similarly; style `.stat-label` in `--text-secondary`
    - Style `.stat-icon` with `color: var(--accent-rose); font-size: 2rem`
    - _Requirements: 8.4, 8.5, 3.1_
  - [x] 6.3 Write `animateCounter` JavaScript function
    - Implement `animateCounter(el, target, duration)` using `requestAnimationFrame`; calculate elapsed time each frame, compute current value as `Math.round(target * (elapsed / duration))`, clamp to target, update `el.textContent`, stop when elapsed >= duration
    - Include fallback to `setTimeout` with 16ms interval when `requestAnimationFrame` is unavailable
    - Wire up: when the stats section becomes visible (via Intersection Observer), call `animateCounter` for each `.stat-number` element using its `data-count` and `data-duration` attributes
    - _Requirements: 8.3_
  - [ ]* 6.4 Write property test for counter animation (Property 1)
    - **Property 1: Counter animation reaches target**
    - Use fast-check: `fc.integer({ min: 1, max: 10000000 })` for target, `fc.integer({ min: 1500, max: 2500 })` for duration; mock `requestAnimationFrame` with `jest.useFakeTimers()`; assert `parseInt(el.textContent) === target` after `jest.runAllTimers()`
    - **Validates: Requirements 8.3**

- [ ] 7. Implement the Gallery_Section component
  - [x] 7.1 Write Gallery_Section HTML inside `<section id="gallery" class="section gallery-section">`
    - Add `<h2 class="section-title">Gallery</h2>`
    - Add `<div class="gallery-grid">` with 9 `.gallery-item` divs, each containing `.gallery-placeholder` (`aria-label="Gallery photo N"`) and `.gallery-overlay`
    - _Requirements: 9.1, 9.2, 9.4_
  - [x] 7.2 Write Gallery_Section CSS
    - Style `.gallery-grid` as `display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: var(--space-sm)`
    - Style `.gallery-item` with `aspect-ratio: 1/1`, `overflow: hidden`, `border-radius: var(--radius-md)`, `position: relative`, `cursor: pointer`
    - Assign unique gradient backgrounds to `.gallery-placeholder` elements by cycling through accent-adjacent hues using `:nth-child` selectors
    - Style `.gallery-overlay` as `position: absolute; inset: 0; background: rgba(10,10,15,0.5)` with accent tint; set `opacity: 0; transition: var(--transition-medium)`
    - On `.gallery-item:hover`: `.gallery-placeholder { transform: scale(1.1); transition: var(--transition-fast); }` and `.gallery-overlay { opacity: 1; }`
    - _Requirements: 9.3, 9.5, 3.5_

- [ ] 8. Implement the Contact_Section component
  - [x] 8.1 Write Contact_Section HTML inside `<section id="contact" class="section contact-section">`
    - Add `<h2 class="section-title">Work With Me</h2>` and `<p class="contact-intro">Available for brand collaborations and promotions</p>`
    - Add `<form id="contact-form" novalidate>` with three `.form-group` divs: Name (`<input type="text" id="name" required>`), Email (`<input type="email" id="email" required>`), Message (`<textarea id="message" rows="5" required>`)
    - Each `.form-group` includes a `<label>`, the input/textarea, and `<span class="field-error" role="alert">` with matching `id` for `aria-describedby`
    - Add `<button type="submit" class="btn btn-primary">Send Message</button>` and `<div id="form-success" class="form-success" hidden>Thanks! Your message has been sent.</div>`
    - _Requirements: 10.1, 10.2, 10.5_
  - [x] 8.2 Write Contact_Section CSS
    - Style `#contact-form` as a max-width centered column with `gap: var(--space-sm)`
    - Style `.form-group` inputs and textarea with dark background (`--bg-card`), `--text-primary` text, `border: 1px solid var(--text-muted)`, `border-radius: var(--radius-sm)`, focus ring using `--accent-rose`
    - Style `.field-error` in a warm error color (e.g., `#f87171`), `font-size: 0.8rem`, hidden by default
    - Style `.form-success` with accent color, padding, and `border-radius: var(--radius-sm)`
    - _Requirements: 10.3, 10.4_
  - [x] 8.3 Write `validateForm` JavaScript function and form submission handler
    - Implement `validateForm({ name, email, message })` returning `{ name, email, message, isValid }` — empty/whitespace name → `"Name is required."`, empty/whitespace email → `"Email is required."`, invalid email format → `"Please enter a valid email address."`, empty/whitespace message → `"Message is required."`, all valid → all empty strings and `isValid: true`
    - Add `submit` event listener on `#contact-form`: call `preventDefault()`, call `validateForm`, display per-field errors in `.field-error` spans (toggle `display: block`/`none`), if `isValid` show `#form-success` and clear all fields
    - _Requirements: 10.3, 10.4, 10.6_
  - [ ]* 8.4 Write property test for form validation (Property 2)
    - **Property 2: Form validation rejects any combination of empty fields**
    - Use fast-check with `fc.record` generating combinations where at least one field is empty/whitespace; assert `result.isValid === false` and the corresponding field error is non-empty
    - **Validates: Requirements 10.4, 10.6**

- [x] 9. Implement the Footer and Back_To_Top_Button components
  - Write Footer HTML inside `<footer>`
    - Add `<div class="footer-content">` with `.footer-social` (same icon links as hero) and `<p class="footer-copy">© <span id="footer-year"></span> Aakriti Aryal. All rights reserved.</p>`
  - Write Back_To_Top_Button HTML: `<button id="back-to-top" aria-label="Back to top" class="back-to-top"><i class="fas fa-chevron-up"></i></button>`
  - Write Footer and Back_To_Top CSS
    - Style `footer` with `background: var(--bg-secondary)`, top border in `--text-muted`, centered content
    - Style `.back-to-top` as `position: fixed; bottom: 2rem; right: 2rem; z-index: 999`, circular accent-colored button; default `opacity: 0; pointer-events: none; transform: translateY(20px); transition: var(--transition-fast)`; when `.visible`: `opacity: 1; pointer-events: auto; transform: translateY(0)`
  - Write Footer and Back_To_Top JavaScript
    - Set `document.getElementById('footer-year').textContent = new Date().getFullYear()`
    - Add `scroll` event listener: if `window.scrollY > 300` add `.visible` to `#back-to-top`, else remove it
    - Add `click` listener on `#back-to-top`: call `window.scrollTo({ top: 0, behavior: 'smooth' })`
  - _Requirements: 12.1, 12.2, 12.3, 13.1, 13.2, 13.3, 13.4_

- [ ] 10. Implement scroll animations with Intersection Observer
  - [x] 10.1 Write scroll animation CSS
    - Add `.animate-on-scroll` base styles: `opacity: 0; transform: translateY(30px); transition: opacity var(--transition-slow), transform var(--transition-slow)`
    - Add `.animate-on-scroll.visible` styles: `opacity: 1; transform: translateY(0)`
    - Apply `.animate-on-scroll` class to all `<section>` elements and any other elements that should animate in
    - _Requirements: 14.1, 14.4_
  - [x] 10.2 Write Intersection Observer JavaScript
    - Implement `createScrollObserver()` factory: creates an `IntersectionObserver` with `threshold: 0.1`; callback adds `.visible` to each intersecting entry's target then calls `observer.unobserve(entry.target)` (one-shot, no replay)
    - Add feature-detection guard: if `'IntersectionObserver' in window` set up observer and observe all `.animate-on-scroll` elements; else immediately add `.visible` to all of them
    - Wire stats counter trigger: inside the observer callback, if the intersecting element is `#stats`, call `animateCounter` for each stat number
    - _Requirements: 14.2, 14.3_
  - [ ]* 10.3 Write property test for scroll animation (Property 3)
    - **Property 3: Scroll animation triggers exactly once per element**
    - Use fast-check with `fc.array(fc.boolean(), { minLength: 1, maxLength: 20 })` to generate intersection event sequences; simulate observer callbacks; assert `.visible` class is added at most once and exactly once if any `true` event occurred
    - **Validates: Requirements 14.2, 14.3**

- [x] 11. Checkpoint — Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 12. Implement accessibility and error-handling polish
  - [x] 12.1 Add accessibility attributes throughout the document
    - Ensure all `<img>` elements have non-empty `alt` attributes; ensure all `.gallery-placeholder` and `.about-image-placeholder` elements have `aria-label`
    - Add `aria-describedby` on each form input pointing to its `.field-error` span
    - Ensure all icon-only buttons (`#back-to-top`, `.hamburger`) have `aria-label`
    - Add `:focus-visible` CSS outline (accent color ring) on all interactive elements
    - _Requirements: 15.2, 15.3, 15.4_
  - [x] 12.2 Add CDN failure and media error handling
    - Add system font fallback stack to `font-family` declaration: `'Poppins', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`
    - Add `onerror` handlers on any `<img>` elements to hide the broken image and show the adjacent placeholder
    - _Requirements: 1.3, 15.1_
  - [ ]* 12.3 Write property test for image alt attributes (Property 4)
    - **Property 4: All images have non-empty alt attributes**
    - Use fast-check with `fc.integer` to index into `document.querySelectorAll('img')`; assert each `img` has `alt` attribute with non-empty trimmed value
    - **Validates: Requirements 15.2**
  - [ ]* 12.4 Write property test for text contrast ratio (Property 5)
    - **Property 5: Text contrast ratio meets WCAG AA**
    - Implement `computeContrastRatio(el)` using `getComputedStyle`; use fast-check with `fc.integer` to index into all text elements; assert contrast ratio >= 4.5
    - **Validates: Requirements 15.5**

- [x] 13. Apply section-level layout and shared styles
  - Write shared `.section` CSS: `padding: var(--space-xl) var(--space-md)`, `max-width: 1200px`, centered with `margin: 0 auto`
  - Write `.section-title` CSS: large font size, `font-weight: var(--font-bold)`, `color: var(--text-primary)`, bottom margin, optional accent underline pseudo-element
  - Verify responsive grid breakpoints for Videos, Gallery, and About sections at 768px and 1024px
  - Verify `--bg-secondary` alternating section backgrounds for visual rhythm
  - _Requirements: 2.1, 2.2, 2.3, 2.4, 3.6_

- [x] 14. Final checkpoint — Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

## Notes

- Tasks marked with `*` are optional and can be skipped for faster MVP
- Each task references specific requirements for traceability
- Checkpoints ensure incremental validation
- Property tests (Properties 1–5) validate universal correctness properties using fast-check
- Unit tests validate specific examples and edge cases
- The `animateCounter` and `validateForm` functions should be written as pure/testable functions before being wired into the DOM
