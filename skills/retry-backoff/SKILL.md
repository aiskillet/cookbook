---
name: retry-backoff
description: Retry failures without making outages worse — exponential backoff, jitter, idempotency, and caps. Use when adding retries to network/DB/queue calls or debugging retry storms.
---

# Retry & Backoff

Retries turn transient blips into successes — or turn a small hiccup into a self-inflicted outage. The difference is backoff, jitter, idempotency, and knowing when *not* to retry.

## When to Activate
- Adding retries to any network/DB/queue/API call
- Debugging a "retry storm" or thundering-herd outage
- Designing resilience for a flaky dependency

## Rules
- **Exponential backoff:** wait ~base·2^attempt (e.g. 200ms, 400ms, 800ms…), not a fixed delay.
- **Add jitter** (randomize the delay). Without jitter, all clients retry in sync and hammer the recovering service — jitter spreads the load. This is essential, not optional.
- **Cap attempts and total time** — e.g. 3–5 tries or a deadline. Infinite retries just prolong failure.
- **Only retry the retryable:** timeouts, connection errors, 429, 503. **Never** retry 4xx (400/401/403/404) — the request is wrong; retrying won't help.
- **Only retry idempotent operations.** For non-idempotent writes, use an **idempotency key** so a retry can't double-charge/double-create.
- **Honor `Retry-After`** when the server tells you how long to wait.

## Going further
- **Circuit breaker:** after repeated failures, stop trying for a cooldown so you don't pile onto a down dependency; probe before fully reopening.
- **Budget retries** — cap the *fraction* of traffic that is retries (e.g. ≤10%) to prevent amplification.
- **Make timeouts shorter than the caller's** so retries fit within the overall deadline.

## Anti-patterns
- Fixed-interval retries with no jitter (synchronized stampede).
- Blanket `retry: 3` on the whole test/HTTP layer (hides real bugs/races).
- Retrying non-idempotent POSTs without an idempotency key.

## Checklist
- [ ] Exponential backoff **with jitter**
- [ ] Attempt + total-time caps
- [ ] Retry only transient errors; never 4xx
- [ ] Non-idempotent ops guarded by idempotency keys
- [ ] `Retry-After` honored; circuit breaker for hard-down deps
