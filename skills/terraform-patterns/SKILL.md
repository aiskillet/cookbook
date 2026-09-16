---
name: terraform-patterns
description: Write maintainable, safe Terraform — modules, remote state, plan-before-apply, and no hardcoded secrets. Use when writing/reviewing IaC or debugging drift.
---

# Terraform Patterns

Infrastructure-as-code is only a win if it's readable, reviewable, and safe to apply. Structure for reuse, protect state, and always plan before you apply.

## When to Activate
- Writing or reviewing Terraform / OpenTofu
- Structuring an IaC repo or modules
- Debugging state drift or a scary `apply`

## Core practices
- **Remote, locked, encrypted state** (S3+DynamoDB, TF Cloud, GCS). Never local state for shared infra — it's a single point of failure and a race condition. Enable state locking.
- **`plan` before `apply`, always.** Review the plan in PRs; a plan that destroys/replaces something is a red flag to investigate. Automate plan on PR, apply on merge (gated).
- **Modules for reuse:** encapsulate a logical component (network, service) with clear inputs/outputs. Keep modules small and composable; version them.
- **Separate environments** (dev/staging/prod) via workspaces or separate state — never share prod state with dev.
- **No hardcoded secrets** — pull from a secret manager / vars; never commit credentials or `.tfstate` (it contains secrets).

## Hygiene
- **Pin provider + module versions** for reproducibility.
- **`fmt` + `validate` + a linter** (tflint) in CI; a policy check (OPA/Sentinel) for guardrails.
- **Least-privilege** for the apply role.
- **Tag everything** (owner, env, cost-center) for accountability.
- **Avoid manual console changes** — they cause drift; if it happened, import or reconcile.

## Watch out
- `terraform destroy` and resource replacement in a plan — read plans carefully.
- Storing state in git (it has secrets, and it's not lockable).

## Checklist
- [ ] Remote, locked, encrypted state; not in git
- [ ] Plan reviewed before apply (gated in CI)
- [ ] Reusable, versioned modules; envs separated
- [ ] Provider/module versions pinned; fmt/validate/lint in CI
- [ ] No secrets in code/state; least-privilege apply role
