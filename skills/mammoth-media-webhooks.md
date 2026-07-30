---
name: Subscribe to and verify TokenBot webhooks
description: Register a webhook endpoint, subscribe to events, send a test delivery, and verify the HMAC-SHA256 signature.
api: https://docs.tokenbot.com/home/api-docs/webhooks/overview
operations: [webhooks_add, webhooks_test]
---

# Subscribe to and verify TokenBot webhooks

## Register
1. `webhooks_add` — `POST /v1/webhooks` with `{ "url": "...", "events": ["trade.executed", "copier.started"] }`. Up to 20 events per webhook, or `["*"]` for all 46 event types.
2. `webhooks_test` — send a test event to confirm your endpoint responds `2xx`.

## Verify every delivery
Each POST carries `X-TokenBot-Signature: sha256=<hex>`, `X-TokenBot-Timestamp`, `X-TokenBot-Event`, and `X-TokenBot-Delivery-Id`.
- Recompute `HMAC-SHA256(secret, "${timestamp}.${raw_body}")` and compare in constant time.
- Reject deliveries whose timestamp is older than 5 minutes (replay window).
- **Idempotency:** de-duplicate on `X-TokenBot-Delivery-Id` — deliveries may be retried with exponential backoff.

## Payload shape
`{ "event": "trade.executed", "event_id": "evt_...", "timestamp": "...", "data": { ... } }`
