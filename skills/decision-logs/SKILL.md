---
name: decision-logs
description: Record decisions so the "why" survives — context, options, choice, and consequences. Use when making a non-trivial technical or product decision (ADRs / decision records).
---

# Decision Logs

Six months later nobody remembers *why* you chose Postgres over DynamoDB — and they re-litigate it. A short decision record captures the reasoning at the moment you have it, so future-you (and new teammates) can trust or revisit it.

## When to Activate
- Making a non-trivial, hard-to-reverse choice (architecture, tooling, vendor, API contract)
- A decision you'll be asked to justify later
- Onboarding someone into past choices

## The format (keep it short — one page max)
```
# <Number>. <Title of the decision>
Date: <date>   Status: proposed | accepted | superseded by #N

## Context
What forces are at play? Constraints, requirements, the problem.

## Options considered
- Option A — pros / cons
- Option B — pros / cons

## Decision
What we chose, and the key reason.

## Consequences
What this makes easy, what it makes hard, what we're accepting.
```

## Principles
- **Capture the why, not just the what.** The reasoning is the valuable part; the choice is obvious in hindsight.
- **Write it when you decide**, not later — the trade-offs are fresh and honest now.
- **Record the options you rejected** and why — that's what stops re-litigation.
- **Immutable + append-only.** Don't edit old records; if a decision changes, write a new one that supersedes it.
- **Store them in the repo** (e.g., `docs/decisions/`), versioned with the code they affect.

## Checklist
- [ ] Context, options, decision, consequences all present
- [ ] The *why* is explicit
- [ ] Rejected options recorded with reasons
- [ ] Dated + status; superseded rather than edited
- [ ] Lives in version control near the code
