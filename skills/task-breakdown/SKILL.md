---
name: task-breakdown
description: Break a big, vague task into small, shippable, ordered steps with clear done-criteria. Use when a task feels too big to start or an estimate feels impossible.
---

# Task Breakdown

"Too big to start" means "not broken down yet." Decompose until each piece is small enough to finish in one sitting and you know exactly when it's done.

## When to Activate
- A task feels overwhelming or you don't know where to start
- An estimate feels impossible (the sign it's too coarse)
- Planning a feature or a multi-step change

## How to decompose
1. **State the outcome** — what's true when this is done?
2. **Find a thin slice** — the smallest end-to-end path that delivers *some* value (a walking skeleton), not horizontal layers.
3. **Split until each step is:** finishable in a few hours, independently verifiable, and has a clear done-criterion.
4. **Order by dependency and risk** — do the uncertain/risky part early (it informs the rest); defer polish.
5. **Name the unknowns** — a "spike" to investigate is a valid first task when you can't estimate.

## Principles
- **Vertical slices over horizontal layers.** "Login works end-to-end for one case" beats "build all the DB models."
- **Each step ships or demos something** — momentum and early feedback.
- **If you can't estimate it, it's too big** — split again, or spike it.
- **Done-criteria per step** — "renders the list from the API" not "work on the list."
- **Surface risk first** — the scariest unknown goes at the top.

## Output
An ordered checklist where every item is small, has a done-criterion, and (ideally) is independently shippable.

## Checklist
- [ ] Outcome stated
- [ ] A thin end-to-end slice identified
- [ ] Steps finishable in one sitting, independently verifiable
- [ ] Ordered by dependency + risk (risky first)
- [ ] Unknowns turned into spikes
