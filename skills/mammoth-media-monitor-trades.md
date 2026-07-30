---
name: Monitor trades and react to fills
description: List and inspect trades, pull aggregated analytics, and cancel an open trade on the TokenBot platform.
api: https://docs.tokenbot.com/home/api-docs/rest-api/overview
operations: [list_trades, get_trade, get_trade_analytics, trade_cancel]
---

# Monitor trades and react to fills

## Auth
Send `X-API-Key: tb_live_...` on every request (or use the CLI signed-request path).

## Steps
1. `list_trades` — list trades; filter by `strategyId`, `exchangeAccountId`, `status`, `side`, `limit`, `offset`.
2. `get_trade` — fetch a single trade by `id` (`trd_` prefix).
3. `get_trade_analytics` — aggregated performance metrics (optional `strategyId`).
4. `trade_cancel` — cancel an open trade by `id`.

## React in real time
Prefer webhooks over polling: subscribe to `trade.executed` (and its deprecated alias `trade.filled`), `trade.cancelled`, `trade.failed`, `trade.closed`. See the webhooks skill.

## Conventions & errors
- Offset pagination (`limit` max 100).
- Flat JSON errors; back off on `429 RATE_LIMITED` using `x-ratelimit-reset`.
