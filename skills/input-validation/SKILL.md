---
name: input-validation
description: Validate untrusted input at the boundary — allowlist, parse-don't-validate, and enforce types/limits before it reaches your logic. Use when handling user input, API payloads, or reviewing for injection risks.
---

# Input Validation

All external input is hostile until proven otherwise. Validate at the boundary, convert to trusted types, and let the core assume clean data.

## When to Activate
- Handling user input, API request bodies, query params, file uploads, webhooks
- Reviewing code for injection or bad-data bugs
- Designing an API's request contract

## Core approach: parse, don't validate
Don't sprinkle `if (bad) throw` everywhere. At the edge, **parse** raw input into a typed, guaranteed-valid value (with a schema validator — Zod, Pydantic, etc.). After the boundary, the type proves it's safe; the core never re-checks.

## Rules
- **Allowlist, not denylist.** Define what's *valid* (this enum, this range, this pattern) and reject the rest. Denylists always miss a case.
- **Validate at the trust boundary** — the API handler / CLI parse — not deep inside.
- **Enforce type, format, and limits:** type, length/size caps, numeric ranges, enum membership, required vs optional.
- **Reject, then coerce carefully.** Prefer rejecting malformed input over silently "fixing" it (silent coercion hides bugs).
- **Bound everything** — max array length, max string size, max upload bytes, request timeouts. Unbounded input is a DoS.

## Injection defense (validation ≠ the whole story)
- **SQL:** parameterized queries, always. Never string-concatenate input into SQL.
- **Shell/OS:** avoid shelling out with user input; if unavoidable, pass args as arrays, never a shell string.
- **HTML/XSS:** escape on output by context; don't trust "sanitized" input.
- **Paths:** resolve + confirm the path stays within an allowed dir (block `../`).

## Checklist
- [ ] Input parsed into typed values at the boundary (schema)
- [ ] Allowlist rules: type, length, range, enum, pattern
- [ ] All sizes/counts bounded
- [ ] Malformed input rejected, not silently coerced
- [ ] Parameterized queries; no shell-string injection; output escaped
