---
name: concurrency-patterns
description: Write correct concurrent code — avoid races, deadlocks, and shared-state bugs with the right primitives. Use when adding threads/async, sharing state, or debugging a heisenbug.
---

# Concurrency Patterns

Concurrency bugs are the worst kind: rare, non-deterministic, and invisible in the happy path. The winning move is to **design out shared mutable state**, not to sprinkle locks until it "works."

## When to Activate
- Adding threads, async/await, workers, or parallelism
- Sharing state between concurrent tasks
- Debugging a flaky, timing-dependent "heisenbug"

## The core rule
**Don't share mutable state.** Most concurrency bugs vanish if tasks own their data and communicate by passing messages/immutable values, not by writing to shared memory.

## Patterns (prefer top to bottom)
1. **Isolation** — each task owns its state; no sharing. Nothing to race.
2. **Message passing / queues** — tasks communicate via channels/queues instead of shared variables (actor model).
3. **Immutability** — shared data is read-only; produce new values instead of mutating.
4. **Locks (last resort)** — when you must share, guard with a mutex. Keep critical sections tiny; never do I/O or call out while holding a lock.

## Avoid the classic bugs
- **Race condition:** unsynchronized read-modify-write. Fix with atomics, a lock, or isolation.
- **Deadlock:** two tasks each holding what the other needs. Fix by **always acquiring locks in a consistent global order**, and use timeouts.
- **Check-then-act:** `if (!exists) create()` across tasks — make it atomic (compare-and-swap / upsert).
- **Unawaited async / fire-and-forget** — you lose errors and ordering. Await or track it.

## Async-specific
- **Don't block the event loop** with CPU-bound work — offload to a worker.
- **Bound concurrency** — a semaphore/pool; unbounded parallelism exhausts connections/memory.
- **Propagate cancellation** and timeouts so stuck tasks don't leak.

## Verify
- Concurrency bugs hide from single-run tests — stress-test with many iterations/parallelism, and use race detectors/thread sanitizers where available.

## Checklist
- [ ] Designed to avoid shared mutable state (isolation/messages/immutability)
- [ ] Locks are last resort; critical sections tiny; consistent lock order
- [ ] No check-then-act races; no unawaited async
- [ ] Concurrency bounded; event loop not blocked; cancellation handled
- [ ] Stress-tested / race detector run
