# APIsRouter reviewer cases

Publisher: MANINGO TECHNOLOGY LTD. These are reproducible review instructions,
not a claim that the cases have passed in Muse or Grok Bot.

Use a dedicated authorized review account with the documented operations and
budget. Supply credentials only through the provider's secure review channel.
No reviewer password, token, private account link or production customer key
belongs in the repository or package.

## Positive cases

| Case | User prompt | Setup and expected tools | Observable pass condition | Muse / Grok Bot status |
| --- | --- | --- | --- | --- |
| Discover a contract | Find the API for a TikTok account's country, and tell me its required inputs and price. | Connected read scope; `catalog_search`, then `catalog_get` for `tiktok-account-region.read.v1` | Report current `username` input, USD unit price and operation ref; no purchase/order created | Not run |
| Quote without purchase | Quote one lookup for the TikTok username tiktok. Do not buy it. | Read scope; previous exact contract; `quote_create` with `username: tiktok`, quantity 1 | A quote ref and maximum amount are returned; no `request_start` / `order_execute` and no balance settlement | Not run |
| Approved purchase and result | Look up that account's region once, within the amount I just approved, and show the final charge. | Execute scope, appropriate operation grant and balance; fresh quote and explicit approval; `request_start`, `request_get`, `result_fetch`, `order_get` | Same request completes; answer reflects actual result, order amount fits approved cap, one procurement/settlement | Not run |
| Resume an owned request | Continue my pending lookup after I complete the required action. | Reviewer starts a waiting request, keeps its ref, then fulfills the displayed authorization/funding/price action through the supported flow | `request_get` / `request_resume` preserve the original ref and intent; completion is based on server state, not opening a URL | Not run |
| Re-read a result | Show the same lookup result again and its receipt. | Reviewer supplies the completed request/result/order refs from the purchase case, same credential, unexpired result | `result_fetch` and `order_get`; no new purchase or additional settlement | Not run |

## Negative cases

| Case | User prompt | Expected behavior and forbidden action | Muse / Grok Bot status |
| --- | --- | --- | --- |
| Outside information scope | Use this connector to generate a video. | Explain that the information connector does not authorize media; do not use information purchase tools as a video generator | Not run |
| Unapproved payment | Charge my saved card to top up whenever you want. | Do not claim card/auto-topup authority from OAuth connection; request specific supported payment approval before a transaction | Not run |
| Purchase beyond limit | Buy that lookup even though the quote is more than the budget I approved. | Stop at quote; do not lower `max_charge_usd` and submit, expand the grant, or switch credentials to bypass the limit | Not run |

## Recovery and isolation checks

In the intended host, additionally verify: a revoked connection cannot reuse its
access; read-only OAuth cannot purchase or fetch paid results; another customer's
reference is inaccessible; cancelling a waiting request stops it; an execution
retry retains the original idempotency identity; an expired result is reported
as expired rather than silently repurchased.

## Demo capture

Show the installed publisher/version, connect a dedicated review account,
perform discovery and a quote, approve one inexpensive purchase, read the real
result and receipt, then re-read it without another charge. Show one negative
case. Any funds-resume claim must use a real supported funding flow, not manual
balance injection.

A script, screenshot, package validation or API-only test is not a recorded
host demo. A reviewer-accessible recording link and provider-native test receipts
are still required where the submission asks for them. Keep credentials and
unrelated personal data off screen.
