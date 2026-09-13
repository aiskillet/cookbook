---
name: writing-quality-code
description: Write code that is correct, clear, and change-safe, then prove it with tests that actually pin the behavior. Use when implementing a feature or fix, refactoring, deciding how to structure a function/module, or figuring out what and how to test.
---

# Writing Quality Code (and Testing It)

Quality is not polish added at the end — it's decisions made while writing, then locked in by tests. This skill is the full loop: **make it correct → make it clear → prove it with tests**, in that order. Speed comes from doing these in the right sequence, not from skipping them.

## When to Activate

- Implementing a new feature, endpoint, or fix
- Refactoring existing code
- Deciding how to structure a function, module, or boundary
- Figuring out *what* to test and *how* to write tests that catch real bugs
- Reviewing whether your own change is done and safe to ship

## The core loop

```
1. Clarify the contract      → what goes in, what comes out, what can't happen
2. Write the simplest code   → make it correct and obvious, not clever
3. Test the behavior         → pin the contract + the edges, prove it fails when wrong
4. Refactor under green      → improve names/structure with tests as a safety net
5. Verify the whole change   → run it, run the suite, check the failure paths
```

Do them in order. Writing tests before step 2 (TDD) is great when the contract is clear; writing them right after is fine when you're exploring. **Never ship step 2 without step 3.**

---

## Part 1 — Writing better code

### Start from the contract, not the implementation
Before typing the body, state (in your head or a comment) the function's contract:
- **Inputs** and their valid ranges.
- **Output** for valid inputs.
- **Preconditions** (what the caller must guarantee) and **invariants** (what stays true throughout).
- **What it must NOT do** (no I/O? no mutation of inputs? no throwing for empty input?).

If you can't state the contract crisply, the design is unclear — fix that before coding.

### Make the common case obvious, handle the edges explicitly
```python
# Weak: edge handling tangled into the happy path
def average(nums):
    return sum(nums) / len(nums)   # explodes on []

# Better: edges named and handled up front, happy path clean below
def average(nums: list[float]) -> float:
    if not nums:
        raise ValueError("average() requires at least one number")
    return sum(nums) / len(nums)
```
Guard clauses at the top, happy path at the bottom, no deep nesting.

### Principles that actually move quality (in priority order)
1. **Correct first.** Clever, fast, or elegant code that's wrong is worthless. Get it right, then improve.
2. **Name things for intent.** `remainingRetries`, not `n`. A good name removes the need for a comment.
3. **One responsibility per function.** If describing it needs "and," split it. Small functions are testable functions.
4. **Push side effects to the edges.** Keep the core logic pure (inputs → outputs, no I/O); do I/O in thin outer layers. Pure cores are trivial to test.
5. **Make illegal states unrepresentable.** Use types/enums so bad combinations can't be constructed, instead of validating them everywhere.
6. **Fail fast and loud.** Validate inputs at the boundary; raise clear errors early rather than corrupting state and failing mysteriously later.
7. **Comments explain _why_, code explains _what_.** Delete comments that restate the code; keep the ones that capture a non-obvious reason or trade-off.
8. **Boring beats clever.** Complexity must earn its keep. Prefer the version the next person can read at 2am.

### Handle errors on purpose
- Decide per failure: **recover, propagate, or fail fast** — never silently swallow (`catch {}` is a bug).
- Error messages name what failed and what to do: `"config.timeout must be a positive integer, got -1"`.
- Clean up resources (files, connections, locks) on the error path too, not just the happy path.

---

## Part 2 — Testing that code

A test's job is to **fail when the behavior is wrong**. A test that can't fail is decoration. Aim for tests that are fast, deterministic, and readable.

### What to test (prioritized)
1. **The contract** — for valid input, you get the specified output.
2. **The edges** — empty, single, max, zero, negative, boundary ± 1, unicode/whitespace for strings.
3. **The error paths** — invalid input raises the right error; failures are handled as designed.
4. **The regression** — every bug you fix gets a test that reproduces it, so it can't come back.
5. **The critical integrations** — the seams where your code meets the DB/network/filesystem (a few, not everything).

Don't chase 100% coverage. Chase coverage of **behavior that would hurt if it broke.** Coverage is a hint, not a goal.

### What makes a good test
- **Arrange–Act–Assert**, visibly. One logical behavior per test.
- **Descriptive name** = the spec: `average_of_empty_list_raises_valueerror`.
- **Deterministic:** no real network, no real clock, no random without a fixed seed, no order dependence.
- **Asserts the outcome that matters,** not incidental internals. Test behavior, not implementation — so refactors don't break tests.
- **Would fail if the code were wrong.** Sanity-check by temporarily breaking the code; if the test still passes, it's testing nothing.

```python
def test_average_of_empty_list_raises():
    with pytest.raises(ValueError, match="at least one number"):
        average([])

def test_average_computes_mean():
    assert average([2, 4, 6]) == 4

def test_average_single_element():
    assert average([7]) == 7
```

### The test pyramid (spend your time here)
- **Many unit tests** — fast, pinpoint failures, cover logic and edges. This is most of your suite.
- **Some integration tests** — real seams (DB queries, API handlers) wired together.
- **Few end-to-end tests** — the handful of critical user journeys. Slow and brittle; keep them scarce and high-value.

Inverting this (mostly E2E) gives a slow suite that fails vaguely — avoid it.

### Test doubles — use the lightest one that works
- Prefer **real objects**; fake only what's slow, non-deterministic, or has side effects (network, time, payments).
- **Stub** to supply canned inputs; **mock** only to assert an interaction actually happened.
- Over-mocking couples tests to implementation and hides real bugs. If a test is 80% mock setup, test at a higher level instead.

---

## Part 3 — Verify the whole change

Writing and testing a function isn't "done." Before you call it finished:

- [ ] **Run the actual code path**, not just the unit tests — exercise it the way a user/caller will.
- [ ] **Run the full suite** — your change didn't break something elsewhere.
- [ ] **Try a failure on purpose** — feed bad input / kill the dependency; confirm it fails the way you designed.
- [ ] **Re-read your own diff** as a reviewer (see the `code-review` skill) — correctness, security, clarity.
- [ ] **Confirm it matches the original contract/requirement**, not something adjacent you drifted into.

> "It compiles" and "the happy path worked once" are not verification. You've verified when you've seen it succeed *and* seen it fail correctly.

---

## Worked example (the loop end to end)

Task: "parse a `page` query param into an int, default 1, reject values < 1."

```python
# 1. Contract: str|None -> int >= 1; missing -> 1; invalid or < 1 -> ValueError
def parse_page(raw: str | None) -> int:
    if raw is None or raw == "":
        return 1
    try:
        page = int(raw)
    except ValueError:
        raise ValueError(f"page must be an integer, got {raw!r}")
    if page < 1:
        raise ValueError(f"page must be >= 1, got {page}")
    return page
```
```python
# 3. Tests: contract, defaults, and every edge/error from the contract
def test_missing_defaults_to_1():      assert parse_page(None) == 1
def test_empty_defaults_to_1():        assert parse_page("") == 1
def test_valid_number():               assert parse_page("5") == 5
def test_non_integer_raises():
    with pytest.raises(ValueError, match="must be an integer"):
        parse_page("abc")
def test_below_one_raises():
    with pytest.raises(ValueError, match="must be >= 1"):
        parse_page("0")
```
Notice: the tests fall directly out of the contract. Contract-first coding makes "what to test" obvious.

## Fast checklist

- [ ] Contract (inputs/output/invariants/what-it-must-not-do) stated before coding
- [ ] Guard clauses up front; happy path clean; no deep nesting
- [ ] Intent-revealing names; one responsibility per function; side effects at the edges
- [ ] Errors handled on purpose (recover/propagate/fail-fast), clear messages, resources cleaned up
- [ ] Tests cover contract + edges + error paths + any bug you fixed
- [ ] Each test would fail if the code were wrong; deterministic; tests behavior not internals
- [ ] Whole change verified: ran it, ran the suite, watched it fail correctly, re-read the diff
