---
name: secrets-management
description: Keep credentials out of code and logs — use a secret store, short-lived tokens, least privilege, and rotation. Use when handling API keys, DB passwords, tokens, or reviewing for leaked secrets.
---

# Secrets Management

A secret in your repo is a secret in everyone's repo. Keep credentials out of code, out of logs, and short-lived.

## When to Activate
- Handling API keys, DB passwords, tokens, certs
- Setting up config/env for an app or CI
- Reviewing a diff for leaked secrets

## The rules
- **Never commit secrets.** Not in code, config, or history. Use a secret store (Vault, AWS/GCP Secrets Manager, 1Password, or platform env vars).
- **Inject at runtime** via env vars or a secrets API — the app reads them, they're not baked into the image/artifact.
- **`.env` is git-ignored**; commit a `.env.example` with keys and dummy values.
- **Least privilege** — each credential does the minimum: scoped tokens, read-only where possible, per-service accounts.
- **Short-lived over long-lived** — prefer temporary/rotating credentials (OIDC, STS) over static keys.
- **Rotate regularly** and on any suspected exposure; make rotation a routine, not a fire drill.
- **Never log secrets** — scrub them from logs, error messages, and traces. Assume logs are widely readable.

## If a secret leaks
1. **Rotate/revoke immediately** — the leaked value is compromised forever.
2. Removing it from git history is **not enough** (it's cached/cloned) — rotation is the real fix.
3. Check access logs for misuse.

## Defense in depth
- Scan for secrets in CI (gitleaks/trufflehog) and pre-commit hooks.
- Limit who/what can read each secret; audit access.
- Separate secrets per environment (dev/staging/prod never share).

## Checklist
- [ ] No secrets in code, config, or history
- [ ] Loaded from a store/env at runtime
- [ ] `.env` ignored; `.env.example` committed
- [ ] Scoped, least-privilege, short-lived where possible
- [ ] Never logged; scrubbed from errors
- [ ] Secret scanning in CI / pre-commit
