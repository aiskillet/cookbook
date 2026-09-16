---
name: load-testing
description: Find capacity and breaking points before users do — realistic scenarios, ramp profiles, and the right metrics. Use before a launch, capacity planning, or after perf changes.
---

# Load Testing

Load testing answers "will it hold up?" before real users find out it won't. The value is in realistic scenarios and reading the results — not in generating a big number.

## When to Activate
- Before a launch or expected traffic spike
- Capacity planning / sizing infrastructure
- Validating a performance change or a scaling setup

## Design realistic tests
- **Model real user behavior**, not a single endpoint hammered in a loop: realistic mix of actions, think-time between requests, and real-ish data. Synthetic uniform traffic gives misleading results.
- **Define the goal + SLO** first: "sustain 2k rps at p95 < 300ms." Test against a target, not "how fast can we go blindly."
- **Test against production-like infra** (same sizes, data volume, dependencies). A test on a laptop tells you nothing about prod.

## Types of test
- **Load:** expected peak — does it meet SLOs?
- **Stress:** ramp past capacity to find the **breaking point** and how it fails (graceful vs cascading).
- **Soak/endurance:** sustained load for hours to catch leaks and slow degradation.
- **Spike:** sudden surge — does autoscaling/queueing cope?

## Read the results
- **Percentiles, not averages** (p95/p99) — averages hide the pain users feel.
- Watch **saturation**: CPU, memory, connections, DB pool, queue depth — find the *first* thing that saturates (the bottleneck).
- Note where errors start and whether failure is graceful (shed load / 429) or a cascade.

## Practically
- Ramp up gradually; establish a baseline; change one variable at a time.
- Use a real tool (k6, Locust, Gatling); keep tests in version control; run regularly, not once.

## Checklist
- [ ] Realistic scenario (action mix + think-time) against SLO
- [ ] Production-like environment and data
- [ ] Load / stress / soak / spike as appropriate
- [ ] Results read as p95/p99 + saturation/bottleneck
- [ ] Failure mode observed (graceful vs cascade)
