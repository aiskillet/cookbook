---
name: refactoring-patterns
description: Improve code structure without changing behavior — safe, incremental moves backed by tests. Use when code is hard to change, before adding a feature to messy code, or when reviewing a large diff.
---

# Refactoring Patterns

Refactoring changes *structure*, never *behavior*. If behavior changes, that's a feature or a bug — not a refactor. The safety net is tests: refactor only under green.

## When to Activate
- Code is hard to read or change
- About to add a feature to a messy area (refactor first, then add)
- A function/file has grown too big
- Reviewing a diff that mixes cleanup with logic changes

## The golden rule
**Never refactor and change behavior in the same commit.** Separate them so review and `git bisect` stay sane.

## High-value moves (in rough priority)
1. **Rename for intent** — the cheapest, highest-impact refactor. `d` → `daysUntilExpiry`.
2. **Extract function** — name a block that needs a comment to explain it.
3. **Guard clauses** — replace nested `if/else` with early returns; flatten the arrow.
4. **Replace magic values** with named constants.
5. **Introduce a type/enum** to make illegal states unrepresentable.
6. **Split by responsibility** — a function described with "and" wants to be two.
7. **Remove dead code** — delete it; git remembers.

## Do it safely
- **Tests green before and after** each step. No tests? Add characterization tests first (pin current behavior), then refactor.
- **Small steps, commit often.** Big-bang rewrites are where bugs hide.
- **One kind of change per commit** — rename OR extract OR move, not all three.
- **Lean on tooling** (IDE rename/extract) over manual edits — fewer mistakes.

## Know when to stop
Refactor to make the *next* change easy, not to chase perfection. If it's readable and testable, ship it.

## Checklist
- [ ] Behavior unchanged (tests prove it)
- [ ] Refactor commit separate from feature/bugfix
- [ ] Small, reversible steps
- [ ] Names reveal intent; nesting reduced
- [ ] Dead code removed, not commented out
