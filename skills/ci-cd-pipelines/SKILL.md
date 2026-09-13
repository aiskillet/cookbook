---
name: ci-cd-pipelines
description: Design fast, reliable CI/CD — cache dependencies, fail fast, keep it reproducible, and gate deploys safely. Use when writing or reviewing a pipeline, or when CI is slow or flaky.
---

# CI/CD Pipelines

CI's job: catch problems fast and give a trustworthy green. CD's job: ship safely and reversibly. A slow or flaky pipeline is worse than none — people stop trusting it.

## When to Activate
- Writing or reviewing a CI/CD config
- CI is slow, flaky, or nobody trusts the checks
- Setting up deploys/gates for a service

## CI principles
- **Fail fast, cheapest-first:** lint → typecheck → unit → integration → e2e. Don't run a 20-min e2e suite if lint fails in 5s.
- **Cache dependencies** (lockfile-keyed) — usually the biggest speedup.
- **Parallelize** independent jobs; shard slow test suites.
- **Reproducible:** pin tool versions; same result locally and in CI. No "works on CI only."
- **Kill flakiness at the source** — quarantine flaky tests, don't blanket-retry (retries hide real races).
- **Keep it fast** — target < 10 min for PR feedback; developers context-switch past that.

## CD principles
- **Build once, promote the same artifact** through environments — never rebuild per env.
- **Gate production** behind required checks + (for risky changes) a manual approval.
- **Make deploys reversible** — one-click rollback / instant revert. This is what lets you move fast.
- **Roll out gradually** for risky changes (canary/blue-green) and watch metrics.
- **Secrets from a secret store**, never in the config or logs.

## Smells to fix
- A single 30-minute monolithic job → split + cache.
- `retries: 3` on the whole suite → find the flaky test.
- Rebuilding the image separately for staging and prod → build once, promote.

## Checklist
- [ ] Stages ordered cheap→expensive; fails fast
- [ ] Dependency cache keyed on lockfile
- [ ] Tool versions pinned; reproducible
- [ ] Same artifact promoted across envs
- [ ] Production gated; rollback is one step
- [ ] Secrets from a store, absent from logs
