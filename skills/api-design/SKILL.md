---
name: api-design
description: Design consistent, evolvable HTTP/JSON APIs — resource modeling, status codes, pagination, filtering, errors, idempotency, and versioning. Use when creating a new endpoint, reviewing an API contract, or deciding how to shape a request/response.
---

# API Design

Opinionated defaults for HTTP/JSON APIs that stay consistent as they grow. When a rule has a trade-off, this skill picks one and tells you why — override deliberately, not by accident.

## When to Activate

- Creating or naming a new endpoint
- Reviewing a request/response shape or an API contract
- Adding pagination, filtering, or sorting to a collection
- Designing error responses or choosing status codes
- Introducing idempotency, versioning, or a breaking change

## The 6 defaults (memorize these)

1. **Resources are plural nouns, kebab-case:** `/invoices`, `/user-sessions`.
2. **Verbs live in the HTTP method, not the URL.** No `/getUser`, `/createInvoice`.
3. **Return the resource you just mutated** (POST/PATCH → the full object), so clients don't need a second GET.
4. **Every list is paginated from day one** — even if today it returns 3 rows.
5. **Errors share one envelope** with a stable machine-readable `code`.
6. **Additive changes only** within a version; anything that removes or renames is a new version.

## Resource & URL design

```
GET    /invoices                 # list (paginated, filterable)
POST   /invoices                 # create → 201 + created resource
GET    /invoices/{id}            # fetch one
PATCH  /invoices/{id}            # partial update → 200 + updated resource
DELETE /invoices/{id}            # delete → 204 (no body)

# Sub-resources for containment, max one level deep
GET    /invoices/{id}/line-items
```

- Nest **at most one level**. Beyond that, use query filters: `/line-items?invoice_id=…`.
- IDs in the path are opaque strings. Prefer ULIDs/UUIDs over sequential integers (no enumeration leaks).
- Actions that aren't CRUD get a **sub-resource verb**: `POST /invoices/{id}/send`, not `POST /sendInvoice`.

## Status codes (the short list you actually need)

| Code | Use for |
|---|---|
| 200 | Success with a body |
| 201 | Created (return the resource + `Location` header) |
| 204 | Success, no body (e.g. DELETE) |
| 400 | Malformed request (bad JSON, wrong types) |
| 401 | Not authenticated |
| 403 | Authenticated but not allowed |
| 404 | Resource doesn't exist (or hidden from this caller) |
| 409 | Conflict (duplicate, version mismatch) |
| 422 | Well-formed but semantically invalid (validation failed) |
| 429 | Rate limited (include `Retry-After`) |
| 500 | You broke, not them — never leak stack traces |

**400 vs 422:** 400 = "I can't parse this." 422 = "I parsed it, but `email` is not a valid email." Keep the split; clients handle them differently.

## Error envelope

One shape everywhere. Clients switch on `code`, show `message`, and use `fields` for form errors.

```json
{
  "error": {
    "code": "validation_failed",
    "message": "One or more fields are invalid.",
    "fields": {
      "email": "must be a valid email address",
      "amount": "must be greater than 0"
    },
    "request_id": "req_01HZ..."
  }
}
```

- `code` is a **stable, documented enum** — never change its meaning.
- Always include a `request_id` so support can trace it in logs.
- Never put human-only prose in `code`; never rely on parsing `message`.

## Pagination

Default to **cursor pagination** for anything that can grow or change under the reader. Use offset only for small, stable, admin-style lists.

```json
GET /invoices?limit=50&cursor=eyJpZCI6...

{
  "data": [ ... ],
  "page": { "next_cursor": "eyJpZCI6...", "has_more": true }
}
```

- Cursors are **opaque** (base64 of internal keyset state). Clients must not construct them.
- `limit` has a **default and a hard max** (e.g. default 25, max 100). Clamp silently, don't 400.
- Offset pagination breaks under inserts/deletes (skips/dupes) — that's why it's the exception.

## Filtering, sorting, sparse fields

```
GET /invoices?status=paid&created_after=2026-01-01&sort=-created_at&fields=id,total
```

- Filters are **explicit query params**, not a free-form query language (that's an injection and support nightmare).
- Sort: comma-separated, `-` prefix = descending. Whitelist sortable fields.
- `fields=` lets clients trim payloads; ignore unknown field names rather than erroring.

## Idempotency & concurrency

- **POST that creates money/side effects** must accept an `Idempotency-Key` header; store the key→response for 24h and replay it on retry. This is how you survive client retries and flaky networks.
- **Concurrent updates:** return an `ETag` on GET; require `If-Match` on PATCH. Mismatch → `409`. This prevents lost updates without pessimistic locking.

## Versioning

- Version in the **URL path**: `/v1/invoices`. It's visible, cacheable, and trivial to route. (Header versioning is "cleaner" in theory and a debugging tax in practice.)
- Within a version, **only additive changes**: new optional fields, new endpoints. Adding a required request field or removing/renaming a response field is **breaking** → new version.
- Treat enums as open: clients must tolerate unknown values so you can add them without a major bump.

## Dates, money, and types (the boring bugs)

- Timestamps: **RFC 3339 UTC strings** (`2026-09-12T18:04:00Z`). Never epoch-seconds-vs-millis ambiguity.
- Money: **integer minor units + currency code** (`{"amount": 4200, "currency": "USD"}`). Never floats.
- Booleans are booleans, not `"true"`. Nulls mean "unset"; omit vs. null should mean the same thing to be safe.

## Pre-ship checklist

- [ ] Plural, kebab-case resource; verb is the method
- [ ] List endpoint is paginated with a hard `limit` cap
- [ ] Mutations return the affected resource
- [ ] All errors use the shared envelope with a stable `code` + `request_id`
- [ ] Correct 400 vs 422 vs 409 semantics
- [ ] Money as minor units; timestamps as RFC 3339 UTC
- [ ] Side-effecting POST accepts `Idempotency-Key`
- [ ] Change is additive within the version (or it's a new version)
