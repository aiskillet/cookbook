---
name: rate-limiting
description: Protect and fairly share an API with rate limits — pick an algorithm, choose keys/limits, and return clear 429s. Use when adding rate limiting or defending against abuse/overload.
---

# Rate Limiting

Rate limiting protects a service from overload and abuse and shares capacity fairly. Get the algorithm, the key, and the response right — and be a good citizen when you're the one being limited.

## When to Activate
- Adding rate limits to an API/endpoint
- Defending against abuse, scraping, or accidental overload
- Consuming a third-party API that rate-limits you

## Pick an algorithm
- **Token bucket** (most common): tokens refill at a rate; each request spends one; allows bursts up to the bucket size. Good default.
- **Sliding window** (counter/log): smooth, accurate limits over a rolling period; avoids the fixed-window edge burst.
- **Fixed window:** simplest, but allows 2× burst at the window boundary — usually avoid for tight limits.
- **Concurrency limit:** cap simultaneous in-flight requests (protects a slow backend).

## Design the policy
- **Choose the key:** per API key / user / IP / tenant. IP alone is weak (shared/NAT, spoofable) — prefer authenticated identity where possible.
- **Set limits by tier/endpoint** — expensive endpoints get tighter limits; paid tiers get more.
- **Distributed limiting:** with multiple servers, keep counters in a shared store (Redis) — per-instance limits don't actually limit.

## Respond well
- Return **429 Too Many Requests** with **`Retry-After`** and rate-limit headers (`X-RateLimit-Limit/Remaining/Reset`) so clients can back off intelligently.
- **Fail open vs closed** deliberately if the limiter store is down.

## When you're the client
- Respect `Retry-After`; back off with jitter; stay under the limit proactively (see [[retry-backoff]]).

## Checklist
- [ ] Algorithm chosen (token bucket / sliding window) for the need
- [ ] Key is authenticated identity where possible; limits per tier/endpoint
- [ ] Distributed counter (shared store) if multi-instance
- [ ] 429 + `Retry-After` + rate-limit headers returned
- [ ] Client side backs off on 429 with jitter
