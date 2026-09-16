---
name: performance-profiling
description: Make code faster by measuring, not guessing — profile, find the real bottleneck, fix it, and confirm. Use when something is slow or you're tempted to "optimize."
---

# Performance Profiling

The first rule of optimization: **measure.** Developers are terrible at guessing bottlenecks, and premature optimization wastes effort on code that isn't hot. Profile first, fix the biggest thing, prove it.

## When to Activate
- Something is slow (endpoint, query, page, job)
- You're tempted to "optimize" a piece of code
- Reviewing a performance regression

## The loop
1. **Set a target + baseline.** "p95 < 200ms." Measure current state so you know if you improved.
2. **Profile under realistic load** — find where time/memory actually goes (a flame graph, timing spans, the query plan). Don't guess.
3. **Fix the biggest bottleneck** — Amdahl's law: optimizing 5% of runtime caps you at 5%. Go for the dominant cost.
4. **Measure again.** Confirm the win; check you didn't regress correctness or another metric.
5. **Stop when you hit the target.** Diminishing returns are real.

## Where the wins usually are (order to check)
- **I/O and queries first** — N+1 queries, missing indexes, chatty network calls, no batching. Almost always the biggest lever, far bigger than CPU micro-opts.
- **Doing work repeatedly** — cache/memoize expensive repeated computation.
- **Algorithmic complexity** — an O(n²) over a big list beats any constant-factor tweak.
- **Payload size** — shipping/serializing more than needed.
- **Only then** CPU-level micro-optimizations.

## Principles
- **Profilers over intuition;** the hot spot is rarely where you think.
- **Optimize the common case**, and the critical path — not rare branches.
- **Correctness first** — a fast wrong answer is worthless; keep tests green.
- **Watch tail latency (p95/p99)**, not just averages.

## Checklist
- [ ] Target + baseline measured
- [ ] Profiled under realistic load; bottleneck identified with data
- [ ] Fixed the dominant cost (I/O/queries checked first)
- [ ] Re-measured; win confirmed; no correctness regression
- [ ] Stopped at the target (no premature micro-opt)
