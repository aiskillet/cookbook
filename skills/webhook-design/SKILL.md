---
name: webhook-design
description: Design webhooks that are reliable and secure — signed payloads, idempotency, retries, and versioning — from both sender and receiver side. Use when building or consuming webhooks.
---

# Webhook Design

Webhooks are HTTP calls to someone else's server over an unreliable network. Design for duplicates, failures, and forgery — because all three will happen.

## When to Activate
- Building a webhook *producer* (you notify others)
- Consuming a *third-party* webhook
- Debugging missed/duplicate webhook deliveries

## Producer side (you send events)
- **Sign every payload** (HMAC of the body with a shared secret) + include a timestamp; let receivers verify authenticity and reject replays.
- **Include a stable `event_id`** so receivers can dedupe.
- **Retry with backoff** on non-2xx, for a bounded window; then dead-letter and surface it.
- **Version your payloads** and keep them additive; breaking changes = new version.
- **Send minimal, stable data** + an ID to fetch the full resource (payloads get logged everywhere).
- **Deliver fast, process async** — don't block the event on your own slow work.

## Consumer side (you receive events)
- **Verify the signature** before doing anything. Reject unsigned/stale requests.
- **Respond 2xx fast** (just acknowledge); do the real work asynchronously. Slow handlers cause retries/timeouts.
- **Be idempotent** — you *will* get duplicates. Dedupe on `event_id`; make processing safe to repeat.
- **Don't trust ordering** — events can arrive out of order; use timestamps/state, not arrival order.
- **Return the right codes:** 2xx = got it (stop retrying); 4xx = won't ever work (stop); 5xx = retry.

## Checklist
- [ ] Payloads signed (HMAC + timestamp); verified on receipt
- [ ] Stable `event_id`; consumer dedupes (idempotent)
- [ ] Ack fast (2xx), process async
- [ ] Retries with backoff + dead-letter (producer)
- [ ] No assumption of ordering; payloads versioned
