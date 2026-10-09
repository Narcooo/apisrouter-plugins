# APIsRouter connector service contract

Publisher: **MANINGO TECHNOLOGY LTD**. Product: **APIsRouter**.
Reviewed against the canonical service implementation on 2026-10-09.

## Service and authentication

- Website: <https://apisrouter.com>
- Support: <https://apisrouter.com/contact> — support@apisrouter.com
- Privacy: <https://apisrouter.com/privacy>
- Product terms: <https://apisrouter.com/terms>
- Canonical remote MCP: `https://api.apisrouter.com/mcp`
- REST/OpenAPI: <https://api.apisrouter.com/v1/information/openapi.json>
- Public discovery: <https://api.apisrouter.com/v1/information/skill>
- Protected-resource metadata: <https://api.apisrouter.com/.well-known/oauth-protected-resource/mcp>
- Authorization-server metadata: <https://api.apisrouter.com/.well-known/oauth-authorization-server/v1/information/oauth>

Each customer signs into their own APIsRouter account. OAuth supports PKCE S256,
dynamic client registration and client ID metadata documents. The host retains
its OAuth credentials. No shared customer key is shipped in this package.

Some hosts can instead create a native REST/API-key connection. Collect the
customer's key only through the host's secure credential form. OAuth scope names
do not apply to an ordinary key; use that key's actual permissions and limits.
This alternative does not authorize silently charging a card.

`information:read` permits discovery, quotes and permitted status/order access.
`information:execute` is also required for purchase, resume, cancellation and
purchased-result retrieval. The customer controls operation scope, spending
limit and expiry. Account connection, permission to spend balance and permission
to charge a payment method are separate. This MCP does not authorize model,
image or video REST calls.

## Tool behavior and proposed review classification

The classifications below are supplied for provider review. Host approval
requirements still apply to every invocation.

| Tool | OAuth requirement | Observable effect | Review classification |
| --- | --- | --- | --- |
| `catalog_search` | read | Searches available operation metadata and USD prices | Read |
| `catalog_get` | read | Reads exact inputs, price, license and result contract | Read |
| `quote_create` | read | Persists an expiring quote; no supplier request or charge | Write, no purchase |
| `request_start` | execute | Creates or reuses one durable request; can procure data and spend balance | Sensitive write: purchase |
| `request_get` | read | Reads the credential's original request and continuation state | Read |
| `request_resume` | execute | Rechecks authorization, price and funds; can continue procurement and spend balance | Sensitive write: purchase |
| `request_cancel` | execute | Cancels a waiting request before dispatch | Write |
| `request_funding` | read | Returns the original request's funding URL while waiting; does not charge a card | Read; transactional link subject to host policy |
| `order_get` | read | Reads an owned order and its actual charge | Read |
| `order_execute` | execute | Compatibility purchase of a quoted operation using an idempotency key | Sensitive write: purchase |
| `result_fetch` | execute | Reads an owned persisted result and records access; no new procurement | Write, no new purchase |

Current tool annotations describe side effects; `quote_create` and `result_fetch`
are not marked read-only. Requests and results are bound to their account and
credential. Sharing an account alone does not grant a different credential access
to an earlier purchase.

## Price, funds and recovery

Prices are in USD and discovered per operation. A quote freezes the inputs,
contract, maximum amount and expiry. The agent may start only within the amount
the user approved. Set `max_charge_usd` to the quoted `maximum_amount` when it
fits that approval; if it does not, stop before requesting purchase.

Purchases use prepaid account balance. Funding links lead to APIsRouter's
account checkout; no tool silently debits a saved card. Native host wallet / Link
checkout is not yet an accepted integration. Its availability must not be claimed.
A funding link or browser confirmation is not proof of payment.

Preserve `request_ref`, quote, input and idempotency identity. A waiting request
can be resumed only after the server rechecks the actual state. Do not replace
an executing or unknown-outcome request with another purchase. Read the order for
the final charge and read the existing result without repurchasing. Quote expiry,
revoked authorization, insufficient scope/funds, changed price and supplier
failure remain explicit failures or waiting states.

Empty-result charging, usage limits, result access lifetime and licensing are
operation-specific. Consult the catalog instead of assuming all operations have
identical rules. A failed, refunded or cancelled order must be described by its
actual state; cancellation of a waiting request is not a general refund API.

## Data handling and result shape

The service processes authenticated account/connection identifiers, request
inputs, supplier results and purchase/audit metadata. Procurement forwards the
selected operation's inputs to its configured supplier. The package does not
request arbitrary access to a customer's mailbox, files or full conversation.

Results are under `data.records`. Interpret each delivery record using the
catalog's `result_contract.record_mode`: `document` wraps its value in `data`,
`page` carries items in `items`, and `records` carries business fields directly.
Follow the declared payload and business-item paths; do not assume one wrapper
for every operation. `total` counts delivery records, not nested business rows.

Result access expires according to `result_ttl_seconds` and revocation/ownership
checks. Access expiry is not a statement that every stored byte or billing record
has been physically deleted. The published privacy policy governs account and
operational records; provider-specific retention/deletion attestations must be
completed from verified operating practice.

One current sample, `tiktok-account-region.read.v1`, takes required `username`,
was listed at USD 0.0025 per call on 2026-10-09, and gives result access for 86,400
seconds. Refresh the live contract and quote before use. Its license permits
analysis and derived reports; raw-dataset redistribution needs separate permission.

## Public distribution status

Source/package availability is independent of provider listing approval.
Muse and Grok Bot native workflows remain unverified until a customer can install
through the intended entry, complete OAuth, quote, obtain an approved real result,
inspect the charge and continue the original task. Public directory approval and
host-specific end-to-end evidence must each be recorded separately.
