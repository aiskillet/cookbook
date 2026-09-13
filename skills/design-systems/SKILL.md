---
name: design-systems
description: Build and use a design system — tokens, components, and consistent patterns so UI stays coherent and fast to build. Use when creating components, defining tokens, or reviewing UI consistency.
---

# Design Systems

A design system trades one-off decisions for reusable ones. Get the tokens and component contracts right and every screen becomes faster to build and consistent by default.

## When to Activate
- Starting or scaling a UI
- Creating a reusable component
- Reviewing UI for inconsistency
- Defining colors/spacing/typography

## Foundation: tokens first
Define **design tokens** — named values, not raw hex/px — for color, spacing, typography, radius, shadow, z-index.
- One scale, used everywhere (`space-2`, `color-fg-muted`) → change once, propagate everywhere.
- Semantic over literal: `color-danger`, not `red-500`, so theming/dark mode is trivial.
- Constrain the scale (a spacing ramp of 8–10 steps) — infinite choices create inconsistency.

## Components
- **Design the API, not just the pixels** — props should express intent (`variant="primary"`, `size="sm"`), not implementation.
- **Composition over configuration** — small pieces that combine beat one component with 30 props.
- **States are part of the component:** default, hover, focus, active, disabled, loading, error, empty.
- **Accessibility is built in**, not bolted on (labels, focus, roles) — see the a11y skill.

## Consistency rules
- Reuse before you create — search the system first.
- Deviation needs a reason; if a pattern recurs, promote it into the system.
- Document usage + do/don't for each component.

## Checklist
- [ ] Values come from tokens, not hardcoded
- [ ] Tokens are semantic and on a constrained scale
- [ ] Component API expresses intent; composable
- [ ] All interaction states designed
- [ ] Reused existing patterns; documented new ones
