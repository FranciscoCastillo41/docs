---
paths:
  - "openapi/*.yaml"
  - "openapi/*.yml"
---

# OpenAPI Spec Standards

The file `openapi/v1.yaml` is the SOURCE OF TRUTH for the API.
It is copied from the `market-data-api` repo. Do NOT edit it here.

## Read-Only Policy

- Changes to the API spec go to `market-data-api/api/openapi/v1.yaml` first
- Then copy the updated file into this repo's `openapi/v1.yaml`
- If you need to fix something in the spec, note it for the API repo

CORRECT: "The spec needs a fix — update it in market-data-api first, then sync."
WRONG: Editing openapi/v1.yaml directly in this repo.

## Validation Requirements

Every endpoint in the spec must have:
- `summary` — short description shown in navigation
- `description` — detailed explanation shown on the endpoint page
- `parameters` — all query params and path params with types and descriptions
- `responses` — at least 200 (success) and relevant error codes (400, 401, 429)
- At least one `example` in the response schema

## Data Format Rules

- All financial values in examples must be strings: `"4.39"` not `4.39`
- All date examples must use real dates: `"2023-10-26"` not `"2024-01-01"`
- Response examples must match the API envelope format:
  - Success: `object`, `request_id`, `status`, `results`, `results_count`, `has_more`
  - Error: `type`, `title`, `status`, `detail`, `instance`, `request_id`
- Error responses use content type `application/problem+json`

## Endpoint Coverage

The spec must document these endpoint groups:
- Auth: `GET /v1/ping`
- Treasury: `/v1/treasury/yields`, `/v1/treasury/yields/curve`, `/v1/treasury/yields/spread`
- Macro: `/v1/series/{id}/observations` (including `as_of`), `/v1/series/{id}/revisions`
- Series: `/v1/series`, `/v1/series/{id}`
- Health: `/healthz`, `/readyz`
