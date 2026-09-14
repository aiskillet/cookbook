---
name: responsive-layout
description: Build layouts that work on any screen — mobile-first, fluid, with modern CSS (flexbox/grid, clamp, container queries). Use when building responsive UI or fixing layouts that break on small/large screens.
---

# Responsive Layout

Design for one column on a phone and let it expand, not the reverse. Modern CSS makes most breakpoints unnecessary — reach for fluid techniques before media queries.

## When to Activate
- Building a layout that must work across devices
- A UI that breaks on mobile or looks empty on wide screens
- Choosing between flexbox, grid, and media queries

## Principles
- **Mobile-first.** Start with the single-column layout; add complexity upward with `min-width` queries. It's easier to expand than to cram.
- **Fluid over fixed.** Prefer `%`, `fr`, `min()/max()/clamp()`, and `flex`/`grid` auto-behavior so it adapts *continuously*, not just at breakpoints.
- **Content-driven breakpoints.** Add a breakpoint where the *content* breaks, not at device widths (there is no "standard" device).

## Modern tools (use these first)
- **Flexbox** for one-dimensional rows/columns and distribution.
- **Grid** for two-dimensional layouts; `repeat(auto-fit, minmax(240px, 1fr))` makes a responsive card grid with **zero media queries**.
- **`clamp(min, preferred, max)`** for fluid type and spacing: `font-size: clamp(1rem, 2.5vw, 1.5rem)`.
- **Container queries** (`@container`) — style a component by *its own* width, so it's reusable in any slot regardless of viewport.

## Rules
- **Never fixed heights** on content that can grow; let it flow.
- **Touch targets ≥ 44px**; readable line length (~45–75 chars) via `max-width` on text.
- **Test the extremes** — 320px wide and an ultra-wide monitor — plus zoom to 200%.
- **`box-sizing: border-box`** globally; use relative units (`rem`) for scalability.

## Checklist
- [ ] Mobile-first; complexity added with min-width
- [ ] Fluid units (clamp/fr/%) before media queries
- [ ] Grid `auto-fit/minmax` for card grids
- [ ] Breakpoints where content breaks, not device sizes
- [ ] Tested at 320px, wide, and 200% zoom; touch targets ≥44px
