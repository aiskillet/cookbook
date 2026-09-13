---
name: error-handling
description: Handle errors deliberately — decide recover vs propagate vs fail-fast, write actionable messages, and never swallow exceptions. Use when adding try/catch, designing error types, or reviewing failure paths.
---

# Error Handling

Errors are part of the contract, not an afterthought. For every failure, make a deliberate choice: **recover, propagate, or fail fast** — never silently swallow.

## When to Activate
- Adding try/catch or error returns
- Designing error/exception types
- Reviewing what happens when a dependency fails
- Debugging a "silent failure"

## The decision for every failure
1. **Recover** — you can meaningfully continue (retry, fallback, default). Do it, and log that you did.
2. **Propagate** — the caller knows better than you. Add context, rethrow/return.
3. **Fail fast** — the state is invalid and continuing corrupts data. Stop loudly.

Swallowing (`catch {}`) is a bug: it hides the failure and defers the crash to somewhere confusing.

## Rules that prevent real bugs
- **Add context as you propagate:** `throw new Error("loading config", { cause: e })` — a stack of "what I was doing" beats a bare error.
- **Messages name the what + the fix:** `"timeout must be a positive integer, got -1"`, not `"invalid input"`.
- **Never catch what you can't handle.** Catch the *specific* error you expect; let the rest bubble.
- **Clean up on the error path too** — release files/locks/connections in `finally`, not just the happy path.
- **Don't log-and-rethrow** the same error at every layer — you'll get 5 copies. Log once, at the boundary that decides.
- **Fail fast on programmer errors** (nulls, bad args); **handle operational errors** (network, disk) gracefully.

## Boundaries
Validate untrusted input at the edge (API handler, CLI parse) and convert it into typed errors. Inside the core, assume inputs are valid — don't re-check everywhere.

## Checklist
- [ ] Every failure: recover / propagate / fail-fast chosen on purpose
- [ ] No empty catch blocks; catches are specific
- [ ] Messages state what failed and what to do
- [ ] Context added when propagating (`cause`)
- [ ] Resources cleaned up on error paths
- [ ] Logged once, at the deciding boundary
