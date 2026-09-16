---
name: component-api-design
description: Design UI component APIs (props) that are intuitive, composable, and hard to misuse. Use when building a reusable component or reviewing a component's props/interface.
---

# Component API Design

A component's props are its API — and like any API, a bad one spreads pain everywhere it's used. Design for intent, composition, and correct-by-default.

## When to Activate
- Building a reusable UI component
- Reviewing a component's props/interface
- A component that's grown a confusing pile of props

## Principles
- **Props express intent, not implementation:** `variant="danger"`, `size="sm"` — not `isRed` or `paddingPx`.
- **Composition over configuration.** Small components that combine beat one mega-component with 30 boolean props. If you're adding the 8th `show*` flag, you probably want composition (children/slots).
- **Sensible defaults; correct by default.** The component should do the right thing with minimal props. Make the common case zero-config.
- **Make illegal states unrepresentable.** Use a `variant` union instead of multiple booleans that can contradict (`isPrimary` + `isSecondary`). Types should forbid nonsense.
- **Controlled vs uncontrolled:** support both where it matters (value + defaultValue), following platform conventions.
- **Don't leak internals** — no exposing DOM/impl details in props; consumers shouldn't depend on how it's built.
- **Consistent naming** across the library (`onChange`, `disabled`, `size`) — predictability reduces docs-reading.

## Smells
- Boolean explosion (`isX`, `hasY`, `showZ`) → use variants/composition.
- Props that only make sense together / contradict → model with a union.
- Passing whole config objects the component picks apart → pass what it needs.
- A prop for every visual tweak → tokens + variants instead.

## Checklist
- [ ] Props name intent, not implementation
- [ ] Composition (children/slots) over flag soup
- [ ] Good defaults; common case is near-zero-config
- [ ] Illegal states unrepresentable (unions over loose booleans)
- [ ] Controlled/uncontrolled handled; naming consistent
