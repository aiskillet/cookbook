---
name: oauth-flows
description: Implement OAuth 2.0 correctly — pick the right flow, use PKCE, and store tokens safely. Use when adding "sign in with…", third-party API auth, or reviewing an OAuth integration.
---

# OAuth 2.0 Flows

OAuth is easy to get subtly wrong in ways that are exploitable. Pick the right flow for your client type, use PKCE, validate everything, and never treat tokens casually.

## When to Activate
- Adding "Sign in with Google/GitHub/…" or third-party API access
- Choosing an OAuth/OIDC flow
- Reviewing an existing OAuth integration for security

## Pick the right flow
- **Authorization Code + PKCE** — the default for web apps, SPAs, mobile, and CLIs. Use this unless you have a specific reason not to.
- **Client Credentials** — machine-to-machine (no user), server holds the secret.
- **Avoid Implicit and Password (ROPC) flows** — deprecated and insecure; don't use them for new work.

## Do it safely
- **Always use PKCE** (even for confidential clients) — defeats auth-code interception.
- **Validate `state`** on the callback — CSRF protection. Generate it, store it, compare it.
- **Exact redirect-URI matching** — register and match exactly; no wildcards/open redirects.
- **Request minimal scopes** — least privilege; add scopes only when needed.
- **Verify ID tokens** (OIDC) — signature, `iss`, `aud`, `exp`, `nonce`. Don't trust an unverified JWT.
- **Exchange the code server-side** where possible; keep the client secret off the client.

## Tokens
- **Access tokens: short-lived**; use **refresh tokens** to renew.
- **Store securely** — httpOnly secure cookies for web; the platform keychain for native; never in localStorage for sensitive tokens (XSS-readable).
- **Rotate refresh tokens** and support revocation.

## Checklist
- [ ] Authorization Code + PKCE (not implicit/password)
- [ ] `state` validated; redirect URI matched exactly
- [ ] Minimal scopes; ID token fully verified
- [ ] Code exchanged server-side; secret not exposed
- [ ] Short-lived access tokens; refresh tokens stored securely + rotatable
