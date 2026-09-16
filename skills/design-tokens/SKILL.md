---
name: design-tokens
description: Define and use design tokens — named, semantic values for color, space, type, and more — so UI stays consistent and themeable. Use when setting up a token system or theming.
---

# Design Tokens

Design tokens are the single source of truth for visual decisions — named values instead of hardcoded hex/px. Get them right and consistency, theming, and dark mode become almost free.

## When to Activate
- Setting up or refactoring a design system's values
- Adding theming / dark mode
- Reviewing UI that hardcodes colors/spacing

## Structure tokens in tiers
1. **Primitive (raw):** the palette — `blue-500: #3b82f6`, `space-4: 16px`. Never used directly in components.
2. **Semantic (aliases):** meaning-based — `color-fg-default`, `color-bg-danger`, `space-inset-md`. Components use *these*.
3. **Component (optional):** `button-bg-primary` → references a semantic token.

Semantic layer is the key: change what `color-danger` maps to and every danger state updates; swap primitives for dark mode without touching components.

## Rules
- **Never hardcode** a raw value in a component — always a token.
- **Semantic > literal:** `color-danger`, not `red`. The name says intent, so theming/rebrand is a token change, not a find-replace.
- **Constrain scales** — a spacing ramp of ~8 steps, a type scale of ~7. Infinite choices = inconsistency.
- **One name per concept**, documented.
- **Tokens are platform-agnostic** — define once (JSON/Style Dictionary), generate CSS vars, iOS, Android, etc.

## Theming
Dark mode / brands = a different mapping of semantic → primitive tokens. Because components only reference semantic tokens, no component code changes.

## Checklist
- [ ] Primitive → semantic → (component) tiers
- [ ] Components reference semantic tokens only; nothing hardcoded
- [ ] Names express intent, not appearance
- [ ] Scales constrained and documented
- [ ] Theming works by remapping semantic tokens
