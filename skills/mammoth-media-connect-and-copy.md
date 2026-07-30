---
name: Connect an exchange and start copy-trading a strategy
description: Validate and register an exchange account, create a strategy, attach a copier with an allocation, and activate it — the TokenBot happy path.
api: https://docs.tokenbot.com/home/api-docs/rest-api/overview
operations: [exchange_test, exchange_add, strategy_create, copier_create, strategy_link_copiers, copier_start]
---

# Connect an exchange and start copy-trading

Use the TokenBot MCP tools (or the equivalent REST endpoints under `https://api.tokenbot.com/v1`) to onboard an exchange and begin copy-trading.

## Auth
- REST: send `X-API-Key: tb_live_...` (or `tb_test_...` against `api-dev.tokenbot.com`). Bearer works too and is treated as an API key.
- The `tokenbot` CLI signs requests with a secp256k1 keypair instead.
- TokenBot API keys only carry **trade** permissions — never withdrawal.

## Steps
1. `exchange_test` — validate the exchange API credentials before saving.
2. `exchange_add` — register the exchange account (returns an `exc_` id).
3. `strategy_create` — create the strategy to follow (returns a `str_` id).
4. `copier_create` — create a copier bound to `strategy_id` + `exchange_account_id`, with an `allocation` 0–100 (returns a `cop_` id).
5. `strategy_link_copiers` — attach the copier(s) to the strategy.
6. `copier_start` — activate the copier so it mirrors trades.

## Conventions & errors
- Pagination is offset-based (`limit` max 100, `offset`).
- Errors are flat JSON `{statusCode, code, message, details}` — handle `VALIDATION_ERROR` (400), `UNAUTHENTICATED` (401), `FORBIDDEN` (403), `RATE_LIMITED` (429).
- Authenticated rate limit is 100 req/min; honor `x-ratelimit-*` headers and back off on 429.
