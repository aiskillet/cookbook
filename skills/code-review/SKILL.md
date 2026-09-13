---
name: code-review
description: Review a diff for correctness, security, and clarity — in priority order — and leave specific, actionable comments. Use when reviewing a pull request, reviewing your own changes before pushing, or when asked to critique code.
---

# Code Review

A review is not "read every line and comment on style." It's a triaged hunt for the things that actually cost you later: bugs, security holes, and code the next person can't safely change. Review in priority order and stop escalating effort once the high-value passes are done.

## When to Activate

- Reviewing a pull request or someone else's diff
- Self-reviewing your own changes before pushing/opening a PR
- Asked to critique, audit, or "look over" code
- Deciding whether a change is safe to merge

## Review in priority order (don't skip to nitpicks)

Do these passes top to bottom. A correctness bug outranks every style opinion.

1. **Correctness** — does it do what it claims, including edge cases?
2. **Security** — can input, auth, or data flow be abused?
3. **Failure modes** — what happens when the network/DB/dependency fails?
4. **Clarity & maintainability** — will the next person understand and safely change this?
5. **Tests** — do they actually pin the behavior that matters?
6. **Style/nits** — last, and mark them as optional.

## Pass 1 — Correctness (where most real bugs hide)

- **Boundaries:** empty input, single element, max size, zero, negative, off-by-one on loops/slices.
- **Nulls/undefined:** every value that *can* be absent — is it handled or assumed?
- **Concurrency:** shared state, read-modify-write races, missing `await`, unawaited promises.
- **State & ordering:** does correctness depend on call order or a side effect that isn't guaranteed?
- **The claim vs. the code:** re-read the PR description, then check the diff actually delivers it — not something adjacent.

> Trick: for each conditional, ask "what's the input that takes the *other* branch?" If you can't name it, there may be a missing case or dead code.

## Pass 2 — Security

- **Untrusted input** reaching a query, shell, filesystem path, template, or `eval` → injection. Look for string concatenation into any of these.
- **AuthZ, not just authN:** the user is logged in — but are they allowed to touch *this specific* resource? Missing object-level checks are the #1 real-world API hole.
- **Secrets:** keys/tokens/passwords in code, logs, or error messages.
- **PII in logs** and over-broad error responses that leak internals.
- **Dependencies:** new packages — reputable? pinned? actually needed?

## Pass 3 — Failure modes

- External calls: timeout set? retried safely (idempotent)? failure surfaced or swallowed?
- Partial failure: if step 3 of 5 throws, is the system left consistent?
- Resource cleanup: files/connections/locks released on the error path, not just the happy path?
- Error handling that hides the error (`catch {}`) is worse than no handling.

## Pass 4 — Clarity & maintainability

- **Names** reveal intent; a comment explaining a bad name means rename instead.
- **Function does one thing;** if you need "and" to describe it, consider splitting.
- **Duplication** that will drift out of sync (vs. incidental similarity — leave that alone).
- **Comments explain _why_,** not _what_. Delete comments that restate the code.
- **Complexity earns its keep** — is the clever version actually needed, or would boring code do?

## Pass 5 — Tests

- Do tests cover the **behavior change**, including the edge cases from Pass 1 — or just the happy path?
- Would the test **fail if the code were wrong?** (Assertions that can't fail are decoration.)
- Tests are readable and don't depend on hidden ordering or real network/time.

## How to write the comment

A good review comment is **specific, actionable, and prioritized.** Vague comments create round-trips.

- ❌ "This could be cleaner."
- ✅ "This N+1 query runs once per row (line 42). Batch with a single `WHERE id IN (…)` before the loop."

Prefix by severity so the author can triage:

- **blocking:** must fix before merge (bug, security, data loss).
- **consider:** worth changing; author's call.
- **nit:** style/preference; never blocks merge.
- **question:** you're unsure — ask before assuming it's wrong.

Rules of engagement:
- Critique the **code, not the person** ("this function" not "you").
- If you request a change, **say what you'd do instead.**
- **Praise real wins** briefly — it calibrates the author on what to keep doing.
- If the diff is huge or the design is wrong, **say so early** — don't line-comment your way through a change that shouldn't exist.

## Approve / block decision

- **Approve** when: correct, secure, failure-safe, and clear enough to maintain. Nits alone never block.
- **Block** when: a correctness/security/data-loss issue, missing tests for risky logic, or you genuinely can't tell if it's correct (unclear code is a maintainability defect).

## Fast checklist

- [ ] Does it do what the PR says, including edge cases?
- [ ] Any untrusted input reaching a query/shell/path? Object-level authz present?
- [ ] External calls have timeouts and safe failure handling?
- [ ] Names/structure clear; no drift-prone duplication?
- [ ] Tests fail if the code is wrong and cover the edge cases?
- [ ] Comments are specific, actionable, and severity-tagged?
