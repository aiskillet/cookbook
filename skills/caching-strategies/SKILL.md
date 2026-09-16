---
name: caching-strategies
description: Cache for speed without serving stale or wrong data — pick the right layer, keys, TTLs, and invalidation. Use when adding a cache or debugging stale/inconsistent data.
---

# Caching Strategies

Caching is the fastest way to speed things up — and the fastest way to serve wrong data. "There are only two hard problems: cache invalidation and naming things." Cache deliberately.

## When to Activate
- A read is slow or expensive and repeats
- Adding a cache layer (in-memory, Redis, CDN, HTTP)
- Debugging stale, inconsistent, or "why is it still showing old data" bugs

## First: should you cache?
Cache when reads ≫ writes, the data is expensive to compute/fetch, and some staleness is acceptable. If data must be always-fresh or changes every read, caching adds bugs without benefit.

## Pick the layer
- **HTTP/CDN** — cache responses at the edge (`Cache-Control`, ETags). Best for public, cacheable GETs.
- **Application cache** (in-memory / Redis) — computed results, DB query results, sessions.
- **Database** — materialized views, query cache.
Cache as close to the consumer as correctness allows.

## The two hard parts
- **Keys:** include everything that changes the result (inputs, user/tenant, version). A too-broad key serves one user another's data (a real security bug); too-narrow kills hit rate.
- **Invalidation:** choose a strategy up front:
  - **TTL (expiry)** — simplest; accept bounded staleness. Great default.
  - **Write-through / write-invalidate** — update or evict on write for freshness.
  - **Versioned keys** — bump a version to invalidate en masse (no delete needed).

## Traps to avoid
- **Stampede/thundering herd** — many misses recompute at once on expiry. Use jittered TTLs, locks, or stale-while-revalidate.
- **Caching per-user data under a shared key** — leaks data across users. Key by identity.
- **Caching errors/empty results** forever — set short negative-cache TTLs.
- **Unbounded caches** — set max size + eviction (LRU); memory leaks otherwise.

## Checklist
- [ ] Reads ≫ writes and staleness acceptable (else don't cache)
- [ ] Layer chosen close to consumer
- [ ] Keys include every result-affecting input (esp. user/tenant)
- [ ] Explicit invalidation: TTL / write-invalidate / versioned
- [ ] Stampede handled; caches bounded; negative caching short
