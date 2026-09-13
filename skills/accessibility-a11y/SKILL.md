---
name: accessibility-a11y
description: Build interfaces everyone can use — semantic HTML, keyboard support, focus, contrast, and screen-reader labels. Use when building UI components or auditing a page for accessibility.
---

# Accessibility (a11y)

Accessible UI is just good UI: it works with a keyboard, a screen reader, and low vision — which also means it works better for everyone. Most of it is free if you start with semantic HTML.

## When to Activate
- Building any interactive UI component
- Auditing a page/app for accessibility
- Reviewing a PR that touches markup or interaction

## Start with semantics (free wins)
- Use the **real element**: `<button>` for actions, `<a href>` for navigation, `<label>` for inputs, headings in order (`h1→h2→h3`). Native elements come with keyboard + a11y built in.
- A `<div onClick>` is not a button — you lose focus, Enter/Space, and screen-reader role.

## The core requirements
- **Keyboard:** everything usable without a mouse. Logical tab order, visible focus ring (don't `outline: none` without a replacement), Escape closes dialogs, Enter/Space activate.
- **Focus management:** move focus into a modal on open, trap it, return it on close.
- **Labels:** every input has a `<label>` or `aria-label`; icon-only buttons have an accessible name.
- **Contrast:** text meets WCAG AA (4.5:1 body, 3:1 large). Don't rely on color alone to convey meaning.
- **Images:** meaningful `alt`; decorative images `alt=""`.
- **ARIA only when needed:** prefer native semantics; use ARIA to fill gaps, and wrong ARIA is worse than none.

## Test it
- Tab through the whole page — can you reach and operate everything?
- Zoom to 200% — does it still work?
- Run an automated check (axe/Lighthouse) — fixes ~40%; the rest is manual.

## Checklist
- [ ] Semantic elements (button/a/label/headings) over generic divs
- [ ] Fully keyboard operable; visible focus
- [ ] All controls have accessible names
- [ ] Contrast meets AA; not color-only
- [ ] Focus managed in dialogs; images have alt
