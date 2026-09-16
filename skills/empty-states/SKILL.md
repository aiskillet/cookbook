---
name: empty-states
description: Design empty, loading, and error states that guide users instead of dead-ending them. Use when building any view that can have no data, be loading, or fail.
---

# Empty, Loading & Error States

The "no data / loading / error" paths are where most UIs feel broken — yet they're an afterthought. Design them as first-class: a blank screen is a lost user.

## When to Activate
- Building any list, dashboard, search, or data view
- A screen shows a blank/spinner/generic error today
- Reviewing UX completeness of a feature

## The states every data view needs
1. **First-use empty** (never had data): explain what goes here + a clear first action. "No projects yet. **Create your first project.**" This is onboarding, not a dead end.
2. **User-cleared empty** (e.g., no search results / filtered to nothing): say why and offer a way out ("No results for 'x' — clear filters").
3. **Loading:** prefer **skeletons** over spinners for content (perceived speed); reserve spinners for actions. Avoid layout shift when data arrives.
4. **Error:** what happened + how to recover (retry button), in plain language, no stack traces. Don't lose the user's input.
5. **Partial/offline:** show what you have; indicate what's stale or failed.

## Principles
- **Never a blank screen.** Every state renders something intentional.
- **Empty = opportunity.** Guide to the primary action; consider a light illustration + one-line value.
- **Recoverable errors:** always a next step (retry, go back, contact) — never a terminal message.
- **Match effort to frequency:** the first-use empty state deserves polish (everyone sees it once); rare errors need clarity, not art.
- **Optimistic where safe**, but reconcile on failure.

## Checklist
- [ ] First-use empty guides to a clear first action
- [ ] No-results/cleared state explains + offers a way out
- [ ] Skeletons for content loading; no layout shift
- [ ] Errors: plain cause + recovery action; input preserved
- [ ] No screen ever renders truly blank
