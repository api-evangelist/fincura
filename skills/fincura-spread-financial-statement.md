---
name: Spread a borrower financial statement
description: Create a borrower, upload a financial document, wait for automated spreading, and retrieve the normalized data view.
api: openapi/fincura-openapi-original.yml
operations: [createBorrower, createDocumentFile, retrieveDocumentFile, retrieveDataViewFromDocumentFile]
---

# Spread a borrower financial statement

Use the Fincura Automated Spreading API to turn a raw financial statement into normalized, spread data.

## Auth
Send `Authorization: Bearer <api_key>` on every request. Keys come from a Fincura user account and refresh via `refreshApiKey` (`POST /v1/api-key/refresh`).

## Steps
1. **Create the borrower** — `createBorrower` (`POST /v1/borrower`). Capture the returned `uuid`.
2. **Upload the document** — `createDocumentFile` (`POST /v1/document-file`) as multipart, referencing `borrower_uuid`. Accepted types: PDF, XLS/XLSX/XLSM, PNG/GIF/JPG. Capture the document `uuid`.
3. **Poll processing status** — `retrieveDocumentFile` (`GET /v1/document-file/{uuid}`). Watch for the terminal state; the same lifecycle is pushed via the `DocumentFile.SpreadComplete` / `DocumentFile.HumanRequired` / `DocumentFile.Error` webhook events. Prefer webhooks over tight polling.
4. **Retrieve the spread** — `retrieveDataViewFromDocumentFile` (`GET /v1/data-view/from_document_file/{document_file_uuid}`) to get the normalized data view.

## Rules
- List endpoints paginate with `limit` / `offset`.
- Only `createWebhook` is documented as idempotent; do not assume other writes are — guard retries yourself.
- Errors are JSON; `429` signals rate limiting (back off).
