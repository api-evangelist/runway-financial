---
name: Export a Runway page as CSV
description: >-
  Export a Runway model page (submodel) or database page as CSV using the Runway
  Export API, optionally from a specific proposal (scenario) layer.
api: openapi/runway-financial-export-api-openapi.yml
operations:
  - exportSubmodel
  - exportDatabasePage
  - exportProposalSubmodel
  - exportProposalDatabasePage
---

# Export a Runway page as CSV

Use Runway's limited Export API to pull a model page (submodel) or database page
out of a Runway workspace as CSV. The API is read-only and GET-only.

## Prerequisites

- An API secret generated in the Runway app under **Settings > API**.
- Export permission on the target page (otherwise the request is denied).
- The `pageID` of the page you want to export (and the `layerID` if you want a
  proposal/scenario view).

## Authentication

Every request sends the API secret as a bearer token:

```
Authorization: Bearer <your API secret>
```

Base URL: `https://runway-api.cfo.ai`

## Steps

1. **Choose the operation** for the page type:
   - Model page (submodel) on Main: `exportSubmodel` — `GET /api/submodel/{pageID}`
   - Database page on Main: `exportDatabasePage` — `GET /api/page/{pageID}`
   - Model page within a proposal layer: `exportProposalSubmodel` —
     `GET /api/proposal/{layerID}/submodel/{pageID}`
   - Database page within a proposal layer: `exportProposalDatabasePage` —
     `GET /api/proposal/{layerID}/page/{pageID}`
2. **Send the GET request** with the `Authorization` header.
3. **Read the JSON response.** It contains `filename` (suggested CSV filename)
   and `contents` (the CSV as a string). Write `contents` to `filename`.

## Rules and gotchas

- Only model (submodel) and database pages are supported. Generic pages return
  an error (see `errors/runway-financial-problem-types.yml`).
- The API is read-only; there are no write/mutating operations and therefore no
  idempotency key is needed (see `conventions/runway-financial-conventions.yml`).
- For a richer read surface (search pages/drivers/scenarios, read page contents,
  calculate driver values), use Runway's hosted MCP server instead
  (`mcp/runway-financial-mcp.yml`), which is OAuth-authenticated and read-only.
