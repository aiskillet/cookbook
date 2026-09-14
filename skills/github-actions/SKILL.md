---
name: github-actions
description: Write GitHub Actions workflows that are fast, secure, and maintainable — caching, least-privilege permissions, pinned actions. Use when creating or reviewing a workflow file.
---

# GitHub Actions

A good workflow is fast (cached, parallel), secure (least privilege, pinned), and readable. Most workflows leak permissions and re-install everything from scratch.

## When to Activate
- Creating or editing `.github/workflows/*.yml`
- CI is slow, or a workflow has broad permissions/secrets
- Reviewing a workflow for security or speed

## Speed
- **Cache dependencies** — use the language setup action's built-in cache (`actions/setup-node` with `cache: npm`) or `actions/cache` keyed on the lockfile.
- **Parallelize** independent jobs; use a **matrix** for versions/OSes.
- **Filter triggers** with `paths:`/`branches:` so you don't run everything on every change.
- **Fail fast** — cheap checks (lint/typecheck) before slow ones.

## Security (this is where most workflows are weak)
- **Least-privilege `permissions:`** — set at the top: `permissions: contents: read`, and grant more only per-job that needs it. The default token is often over-privileged.
- **Pin third-party actions to a full commit SHA**, not a tag (`uses: owner/action@<sha>`). Tags are mutable; a compromised tag = supply-chain attack.
- **Be careful with `pull_request_target`** and untrusted input — never run untrusted PR code with secrets. Don't interpolate untrusted `${{ github.event.* }}` into `run:` (script injection).
- **Secrets** via `secrets:`, never echoed; scope environments with required reviewers for deploys.

## Maintainability
- Reusable workflows (`workflow_call`) / composite actions for repeated logic.
- Concurrency groups to cancel superseded runs: `concurrency: { group: ..., cancel-in-progress: true }`.
- Name steps clearly; keep jobs single-purpose.

## Checklist
- [ ] Dependencies cached; jobs parallel/matrix where useful
- [ ] Top-level `permissions` least-privilege
- [ ] Third-party actions pinned to SHA
- [ ] No untrusted input interpolated into `run:`; secrets never logged
- [ ] Concurrency cancels stale runs; triggers filtered
