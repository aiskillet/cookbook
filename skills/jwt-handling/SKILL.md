---
name: jwt-handling
description: Use JWTs securely — verify signatures and claims, pick the right algorithm, and store/expire tokens safely. Use when implementing token auth or reviewing JWT code.
---

# JWT Handling

JWTs are convenient and easy to use insecurely. The bugs are subtle and catastrophic (auth bypass). Verify everything, trust nothing in the token until the signature checks out.

## When to Activate
- Implementing token-based auth (login, API tokens)
- Reviewing code that issues or verifies JWTs
- Integrating an OIDC/OAuth provider's tokens

## Verify properly (the critical part)
- **Always verify the signature** before reading any claim. An unverified JWT is attacker-controlled data.
- **Pin the algorithm.** Reject `alg: none`, and don't let the token's header choose the algorithm (the classic `alg` confusion / RS256→HS256 attack). Configure the expected alg server-side.
- **Validate the standard claims:** `exp` (not expired), `iat`/`nbf`, `iss` (expected issuer), `aud` (intended for you). A valid signature on a token meant for another service is still an attack.
- **Verify against the right key** — for RS/ES, the issuer's public key (fetch JWKS, cache with rotation); for HS, a strong shared secret.

## Tokens & lifecycle
- **Short-lived access tokens** (minutes); use **refresh tokens** to renew.
- **Don't put secrets/PII in the payload** — it's base64, not encrypted; anyone can read it.
- **Storage:** httpOnly, Secure, SameSite cookies for web (not localStorage — XSS-readable); platform keychain for native.
- **Revocation:** JWTs are stateless and can't be un-issued — keep them short-lived, and for logout/compromise use a denylist or rotate signing keys / refresh tokens.

## Checklist
- [ ] Signature verified before trusting any claim
- [ ] Algorithm pinned; `alg: none` and header-chosen alg rejected
- [ ] `exp`, `iss`, `aud` (and nbf/iat) validated
- [ ] No secrets/PII in payload; short-lived access + refresh tokens
- [ ] Stored in httpOnly/Secure cookies (web); revocation strategy exists
