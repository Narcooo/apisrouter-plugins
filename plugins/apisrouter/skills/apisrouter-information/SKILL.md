---
name: apisrouter-information
description: Buy social, search and web data from the APIsRouter catalog through the apisrouter MCP server. Quote first, buy within the approved limit, read the result, and hand payment to the user when a request waits on funds.
---

# APIsRouter information

The `apisrouter` MCP server exposes a paid catalog of data operations
(social profiles and posts, search results, keyword and site metrics, web
pages, and more). Every purchase is quoted first and charged to the account
holder's prepaid balance within the limit they approved when connecting.

## Workflow

1. `catalog_search` with a short keyword (`query`), optionally `platform`,
   `family` or `category`. Search is a literal substring match: try shorter
   or alternate terms when nothing matches, and page with `offset` instead of
   loading the whole catalog.
2. `catalog_get` for the chosen `operation_ref`. It returns the exact input
   fields, unit price, limits and a result example. Refresh it before quoting.
3. `quote_create` with `operation_ref`, `input` and `quantity`. The quote
   freezes input, contract and price for a few minutes. Nothing is charged.
4. `request_start` with `quote_ref`, a fresh `idempotency_key` and
   `max_charge_usd` as a decimal string no larger than the quote's
   `maximum_amount`. Keep the returned `request_ref`. Retries must reuse the
   same quote and idempotency key; never start a second purchase for the same
   intent.
5. Inspect the request's structured status. Use `request_get` for an executing
   request; stop polling when user action is required. Once `completed`, use `result_fetch` with the
   `result_ref` (`offset` 0, `limit` up to 1000). Read `data.records`. The
   result's `total` counts delivery records, not rows inside a record.

## When a request waits

- `awaiting_payment`: retain the original `request_ref` and explain the missing
  balance. Only where the host permits a transactional link, call
  `request_funding` and use its exact `action_url`, including `request_ref`.
  Do not replace it with a generic recharge URL. On a host that prohibits such
  links, explain the entitlement limit and link only to the informational
  https://apisrouter.com/pricing page. When the user returns, call
  `request_resume` with the original ref; the server checks actual funds.
  Opening a link or a user statement is not payment evidence.
- `awaiting_price_confirmation`: preserve the original request. For OAuth,
  direct the account holder to https://apisrouter.com/console/capabilities
  to review and confirm that request's price. A normal API Key must use the
  authenticated REST review/confirm flow documented by the live OpenAPI;
  do not substitute the account's OAuth confirmation. A chat reply alone
  does not call an unavailable MCP confirmation tool.
- `awaiting_authorization`: the connection's limit or scope does not cover
  this operation. Tell the user to widen it at
  https://apisrouter.com/console/authorizations; do not retry blindly.
- `request_cancel` drops a waiting request. Never cancel or replace a request
  that is `executing` or `outcome_unknown`.

## Rules

- Say what an operation costs before buying it; prices are in USD and shown
  by `catalog_get` and `quote_create`.
- `insufficient_scope` means the connection is read only. The agent can
  search and quote; purchases need the user to reconnect with full access.
- Results are already paid for: re-read them with `result_fetch`, do not buy
  again. Keep `request_ref`, `order_ref` and `result_ref` in your notes.
- Use `order_get` with a known `order_ref` to inspect the final charged amount.
  MCP does not list all account requests: for a request without a known ref,
  ask the user to select it in the account's capability history.
- Do not paste API keys or tokens into prompts, files or logs.

## Without MCP

The same contract is available over REST with an APIsRouter API key:
https://api.apisrouter.com/v1/information/skill describes the endpoints and
https://api.apisrouter.com/v1/information/openapi.json is the OpenAPI document.
