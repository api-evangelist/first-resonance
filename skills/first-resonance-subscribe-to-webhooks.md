---
name: Subscribe to ION realtime webhooks
description: Register a webhook receiver and create a WebhookSubscription so ION pushes realtime events (e.g. RUNS created) to your endpoint.
api: https://api.buildwithion.com/graphql
method: generated
source: https://manual.firstresonance.io/api/webhooks
operations:
- create/register a webhook Receiver (sharedSecret, expectedResponseCode)
- create a WebhookSubscription (receiverId, resource, action)
---

# Subscribe to ION realtime webhooks

Use this to receive realtime event notifications from ION Factory OS.

## 1. Stand up an HTTPS receiver
Webhooks require an HTTPS endpoint. Restrict inbound traffic to ION's published
per-environment **IP allowlist** (staging / production / sandbox).

## 2. Register a Receiver
Via the GraphQL API, create a **Receiver** pointing at your HTTPS URL. Configure:
- `sharedSecret` — shared secret you use to validate the request.
- `expectedResponseCode` — the HTTP status ION should expect back on success.

## 3. Create a WebhookSubscription
Create a **WebhookSubscription** binding the receiver to a resource/action pair:
- `receiverId` — the Receiver you created.
- `resource` — the object type to watch (e.g. `RUNS`).
- `action` — one of `CREATE`, `UPDATE`, `DELETE`.

## 4. Discover all subscribable events
The full list of resources/actions is enumerated in the **Interactive API Explorer**
under the `WebhookSubscription` type — inspect it rather than hardcoding a guessed list.

## Notes
- Deliveries are recorded in an events audit trail.
- Signature verification beyond `sharedSecret` is not documented; validate the shared secret.
- Authenticate the management calls with an ION API-key access token (see the authenticate-and-query skill).
