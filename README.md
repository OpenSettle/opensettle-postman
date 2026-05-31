# opensettle-postman

Postman collection + environment templates for the [OpenSettle](https://opensettle.io)
API. Drop-in for trying the API against your test workspace without
writing any code.

## What's in this repo

- `OpenSettle.postman_collection.json` — collection covering every public
  v1 endpoint, organized by resource (Me, Customers, Products,
  Invoices, Checkouts, Subscriptions, Wallets, Payments, Webhook
  Endpoints, Health). 50+ requests total.
- `OpenSettle.postman_environment.json` — environment template with the
  variables you need: `apiKey`, `workspaceId`, `apiOrigin`. Fill them in
  for your workspace, save under a different name, and you're ready.

Every mutating request has `Idempotency-Key: {{$guid}}` set — Postman
substitutes a fresh UUID per send, so retries from the runner are safe.
The collection-level Bearer auth uses the `apiKey` variable, so you only
set the key once.

## Quickstart (under 2 minutes)

1. **Import**: in Postman, File → Import → drag both JSON files.
2. **Set environment**: top-right environment selector → "OpenSettle".
3. **Fill in your variables**:
   - `apiOrigin` = `https://api.opensettle.io`
   - `apiKey` = your `sk_test_…` key from the dashboard's Developers tab
   - `workspaceId` = your workspace ID (begins with `ws_`)
4. **Run any request**. Start with `GET {{apiOrigin}}/v1/workspaces/{{workspaceId}}/customers` —
   a 200 confirms your `apiKey` and `workspaceId` are correct. (Note:
   `GET /v1/me` is dashboard-session only and will 401 with an API key.)

As you create resources, copy the IDs into the matching collection
variables (`customerId`, `invoiceId`, `priceId`, `subscriptionId`,
`walletId`, `endpointId`, etc.) — every "Get / Update / Delete" request
references them.

## Live spec

The collection is hand-curated to stay readable; the canonical machine
description of every endpoint is the OpenAPI 3.1 spec at
[`/v1/openapi.json`](https://api.opensettle.io/v1/openapi.json), also
published at [OpenSettle/opensettle-openapi](https://github.com/OpenSettle/opensettle-openapi).
If the collection lags behind a new endpoint, the spec is the source of
truth.

## Insomnia / Bruno / Hoppscotch

The OpenAPI spec at the link above imports cleanly into all three. Use
that path if Postman isn't your tool of choice.

## Contributing

Missing endpoint? Wrong header? Stale example? Open an issue or PR. The
collection schema is Postman v2.1.

## License

[MIT](./LICENSE) — copy, modify, redistribute, use commercially. No
warranty.

## Security

Don't publish your live API keys in committed Postman environments.
The `.postman_environment.json` template here ships with empty values
on purpose. For coordinated vulnerability disclosure, see
[opensettle.io/security](https://opensettle.io/security).
