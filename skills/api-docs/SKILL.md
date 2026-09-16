---
name: api-docs
description: Write API docs developers can actually use — quickstart, real request/response examples, auth, errors, and pagination. Use when documenting an API/SDK or reviewing reference docs.
---

# API Documentation

Developers judge an API by its docs in minutes. Great docs get someone to a successful first call fast, then answer every "what happens if…". Show, don't just describe.

## When to Activate
- Documenting a REST/GraphQL API or an SDK
- Reviewing reference docs for completeness
- Writing a getting-started / integration guide

## Structure
1. **Quickstart:** authenticate + make one real call + see a real response, in under 5 minutes. This is the most important page.
2. **Authentication:** how to get keys, how to send them, scopes, and what a 401/403 looks like.
3. **Per-endpoint reference:** method + path, purpose, **all params** (name, type, required, default, constraints), and a **real request + real response** (copy-pasteable, realistic data).
4. **Errors:** the error format, the status codes, and what each means + how to fix.
5. **Cross-cutting:** pagination, rate limits, versioning, idempotency, webhooks.

## Principles
- **Every endpoint has a runnable example** — real curl/SDK snippet + the actual JSON back. Examples beat prose.
- **Document the unhappy paths** — errors, limits, edge cases. That's what devs hit and search for.
- **Be exact and current** — wrong docs are worse than none. Generate from the spec (OpenAPI) where possible so they can't drift.
- **Copy-pasteable** commands that work verbatim, with a way to try it (sandbox/console).
- **State defaults and constraints** for every field; note what's optional vs required.

## Checklist
- [ ] Quickstart: auth → first successful call in <5 min
- [ ] Every endpoint: params (type/required/default) + real request & response
- [ ] Auth, errors, pagination, rate limits, versioning covered
- [ ] Examples are runnable and current (ideally spec-generated)
- [ ] Unhappy paths documented, not just the happy path
