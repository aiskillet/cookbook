---
name: api-client-patterns
description: Build resilient API clients — timeouts, retries with backoff, pagination, and typed errors. Use when integrating a third-party API or writing an SDK/wrapper.
---

# API Client Patterns

A naive API client works in the demo and fails in production the first time the network hiccups. Build for timeouts, retries, and rate limits from the start.

## When to Activate
- Integrating a third-party/HTTP API
- Writing an SDK or internal client wrapper
- Debugging flaky or slow external calls

## The essentials
- **Always set a timeout.** No call is allowed to hang forever — a missing timeout is the #1 cause of cascading outages.
- **Retry idempotent calls with exponential backoff + jitter.** Retry GETs and idempotent writes on 5xx/429/network errors; never blindly retry non-idempotent POSTs (use idempotency keys).
- **Honor `Retry-After`** on 429; respect rate limits proactively rather than hammering.
- **Handle pagination** — follow cursors/next-links; don't assume one page.
- **Fail with typed errors** — distinguish auth (401/403), not-found (404), client (4xx), transient (5xx/timeout) so callers can react correctly.

## Structure
- **One place for cross-cutting concerns** — auth headers, base URL, timeout, retry, logging in a single client, not sprinkled per call.
- **Centralize auth/token refresh** — refresh on 401 once, then retry; don't duplicate refresh logic.
- **Parse into typed models** at the boundary; don't leak raw JSON through the app.
- **Make it testable** — inject the HTTP layer so you can stub responses.

## Observability
- Log request id, status, and latency (never secrets/tokens).
- Surface remaining rate-limit budget if the API exposes it.

## Checklist
- [ ] Timeout on every request
- [ ] Backoff+jitter retries for idempotent/transient failures only
- [ ] `Retry-After` / rate limits respected
- [ ] Pagination handled; responses parsed into typed models
- [ ] Typed error categories; auth/refresh centralized
