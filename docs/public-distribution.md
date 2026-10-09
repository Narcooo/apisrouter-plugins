# Public host distribution

APIsRouter is published by **MANINGO TECHNOLOGY LTD**. Customers should install
one official connector, authorize their own account, and continue their task.
This repository is the public source for that connector; a user's private copy
is a test or installation context, not evidence of a public marketplace listing.

## Grok Bot / Cursor

[Cursor's plugin reference](https://cursor.com/docs/reference/plugins) accepts
public Git repositories containing an Agent Plugin or Cursor Plugin. For this
nested repository, `.cursor-plugin/marketplace.json` points to the package in
`plugins/apisrouter`; its Cursor overlay references the same MCP and Skill.

The [publisher application](https://cursor.com/marketplace/publish) requires a
signed-in account. Prepare the publisher identity, repository, descriptions and
review materials before submitting. Record an actual submission receipt and
approval; do not treat a Git push as marketplace publication.

Grok Bot exposes connectors through its own Plugins / Marketplace and in-chat
Connect cards. Its shared Cursor connector policy is documented in the
[team guide](https://cursor.com/docs/grok-bot/teams). Confirm Grok Bot availability
for the approved listing and test installation there; Cursor package validation
alone does not establish Grok Bot support.

## Muse

[Muse Connector Platform](https://muse.ai/platform) reviews submitted connectors
before making them discoverable. Its form currently has Overview, Technical
Details and Review stages. The overview asks for the product, company, website,
examples, icon, payment category, contact, support, privacy and product terms.

The [connector guidelines](https://muse.ai/platform/docs) require accurate tool
classification, approved transaction behavior, credentials through a secure
review channel and an end-to-end workflow. Submit the service contract and cases
below. State prepaid-balance purchases and the current external funding step
accurately; native Link support and payment-resume completion are not verified.

A separate 2026-10-09 user-flow probe observed Muse read the public operation
contract, return the correct TikTok region input and USD price, and generate a
native custom API connector card. Its Connect flow opened a masked API Key form.
No key was entered, and authenticated quoting, purchasing and continuation were
not exercised. This was one account and a REST/API-key path, not MCP OAuth or a
directory approval. Availability must be checked in the current customer's host.

Where that native capability exists, the public instruction can direct Muse to
the official service contract and let the customer enter their own key only in
Muse's secure form. Do not copy the test account's generated link or credential.
An independent new-user reproduction is still needed before declaring this
route accepted. Formal directory distribution proceeds separately.

## Shared materials

- [Service, permissions, payments and result contract](service-contract.md)
- [Reviewer cases and demo requirements](reviewer-cases.md)
- Product/support: <https://apisrouter.com> / <https://apisrouter.com/contact>
- Policy/terms: <https://apisrouter.com/privacy> / <https://apisrouter.com/terms>
- Operator email: support@apisrouter.com

As of 2026-10-09, public Muse and Grok Bot listings have not been submitted or
approved. Personal-account tests, OAuth discovery and package validation are
tracked separately. No supported countries, commercial attestations, review
credentials or recording URL should be invented to complete a submission.
