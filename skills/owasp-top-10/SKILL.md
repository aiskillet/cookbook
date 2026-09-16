---
name: owasp-top-10
description: Check web apps against the most common, highest-impact security risks (OWASP Top 10) with concrete defenses. Use when building or reviewing a web app/API for security.
---

# OWASP Top 10

The OWASP Top 10 is the industry list of the most common, damaging web security risks. Most real breaches are these basics — not exotic attacks. Cover them.

## When to Activate
- Building or reviewing a web app / API
- A security pass before shipping
- Prioritizing what to harden first

## The risks & defenses
1. **Broken Access Control** — the #1 risk. Enforce authorization on *every* request at the object level (can *this* user touch *this* resource?). Deny by default. Never trust the client for access decisions.
2. **Cryptographic Failures** — encrypt sensitive data in transit (TLS) and at rest; hash passwords with bcrypt/argon2; never roll your own crypto; don't log secrets/PII.
3. **Injection** (SQL/NoSQL/OS/LDAP) — parameterized queries always; never concatenate input into queries/commands; validate + escape by context.
4. **Insecure Design** — threat-model early; build security in, don't bolt on. (See the threat-modeling skill.)
5. **Security Misconfiguration** — no default creds; disable debug in prod; least-privilege; security headers (CSP, HSTS); lock down cloud storage/CORS.
6. **Vulnerable & Outdated Components** — inventory dependencies; scan (SCA); patch known CVEs promptly.
7. **Identification & Authentication Failures** — strong session management, MFA, rate-limit/lockout on login, secure password reset, no session fixation.
8. **Software & Data Integrity Failures** — verify integrity of updates/CI artifacts and third-party code; beware insecure deserialization and unsigned pipelines.
9. **Security Logging & Monitoring Failures** — log auth and security events (no secrets); alert on anomalies; you can't respond to what you can't see.
10. **Server-Side Request Forgery (SSRF)** — validate/allowlist outbound URLs; block requests to internal/metadata endpoints.

## Principles
- **Defense in depth** — no single control; assume each layer can fail.
- **Least privilege** everywhere (users, services, tokens).
- **Never trust input or the client.**

## Checklist
- [ ] Object-level access control on every request; deny by default
- [ ] Parameterized queries; input validated/escaped
- [ ] TLS + hashed passwords; no secrets in logs
- [ ] Dependencies scanned + patched; no default creds/debug in prod
- [ ] Auth hardened (MFA, rate limits); security events logged; SSRF allowlisted
