---
name: Subscribe to Fincura webhook events
description: Register an HMAC-signed webhook to receive document-processing and analysis events.
api: openapi/fincura-openapi-original.yml
operations: [createWebhook, listWebhooks, retrieveWebhook, destroyWebhook]
---

# Subscribe to Fincura webhook events

## Auth
`Authorization: Bearer <api_key>` on every request.

## Steps
1. **Register** — `createWebhook` (`POST /v1/webhook`) with an `event_type`, a `webhook_url`, and a `webhook_method`. Use `*` to receive all events. This call is idempotent. Capture the returned `signing_key`.
2. **Verify deliveries** — validate the HMAC signature on each delivery using the `signing_key`.
3. **List / inspect** — `listWebhooks` (`GET /v1/webhook`), `retrieveWebhook` (`GET /v1/webhook/{uuid}`).
4. **Remove** — `destroyWebhook` (`DELETE /v1/webhook/{uuid}`).

## Event types
`DocumentFile.Received`, `DocumentFile.Processing`, `DocumentFile.HumanRequired`, `DocumentFile.SpreadComplete`, `DocumentFile.Error`, `BulkFile.Received`, `BulkFile.Processing`, `BulkFile.Processed`, `BulkFile.Error`, `Analysis.Saved`, `GlobalCashflow.Saved`, `Template.Changed`, `Borrower.CreatedFromImport`, `CalculatedStatement.Update`, `ForecastedStatement.Update`, `AnnualizedStatement.Update`, `*`.
