---
name: Run a DSCR analysis for a borrower
description: Create a debt-service-coverage-ratio analysis for a borrower and download the PDF/Excel result.
api: openapi/fincura-openapi-original.yml
operations: [listDscrTemplates, createDscrAnalysis, retrieveDscrAnalysis, downloadPdfDscrAnalysis, downloadExcelDscrAnalysis]
---

# Run a DSCR analysis for a borrower

## Auth
`Authorization: Bearer <api_key>` on every request.

## Steps
1. **Pick a template** — `listDscrTemplates` (`GET /v1/template/dscr`) to choose a `template_uuid`.
2. **Create the analysis** — `createDscrAnalysis` (`POST /v1/analysis/dscr`) with the borrower's `borrower_uuid` and the chosen `template_uuid`. Capture the analysis `uuid`.
3. **Retrieve it** — `retrieveDscrAnalysis` (`GET /v1/analysis/dscr/{uuid}`). The `Analysis.Saved` webhook event fires when saved.
4. **Download** — `downloadPdfDscrAnalysis` (`GET /v1/analysis/dscr/{uuid}/download_pdf`) or `downloadExcelDscrAnalysis` (`.../download_excel`).

## Notes
- Global cashflow analysis mirrors this flow under `/v1/analysis/global-cashflow` (`createGlobalCashflowAnalysis`, `retrieveGlobalCashflowAnalysis`, downloads).
- Prerequisite: the borrower must already have spread financials (see the spreading skill).
