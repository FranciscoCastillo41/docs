---
name: sync-spec
user-invocable: true
description: Copy the latest OpenAPI spec from the market-data-api repo
---

# /sync-spec — Sync OpenAPI Spec

Copy the latest `v1.yaml` from the `market-data-api` repo into this docs repo.

## Steps

1. **Check for local API repo**: Look for `../market-data-api/api/openapi/v1.yaml`.
   If the file does not exist, report the error and suggest the user clone
   the API repo or provide the path.

2. **Backup current spec**: If `openapi/v1.yaml` exists, read it to compare later.

3. **Copy the spec**: Copy `../market-data-api/api/openapi/v1.yaml` to `openapi/v1.yaml`.

4. **Validate the new spec**:
   - Ensure the file is valid YAML
   - Check that all previously documented endpoints still exist
   - Flag any endpoints that were removed (breaking change)

5. **Report changes**:
   - New endpoints added
   - Endpoints removed (BREAKING — flag prominently)
   - Modified parameters or response schemas
   - New or changed example values

## Output Format

```
## Spec Sync Report

Source: ../market-data-api/api/openapi/v1.yaml
Target: openapi/v1.yaml

### Changes Detected
- [NEW] GET /v1/series/{id}/metadata — new endpoint
- [MODIFIED] GET /v1/treasury/yields — added "curve_type" parameter
- [REMOVED] GET /v1/old-endpoint — BREAKING CHANGE

### Action Required
- Create documentation for new endpoint: GET /v1/series/{id}/metadata
- Update guides that reference modified parameters
- Remove references to removed endpoints

### No Changes
Spec is identical to the previous version.
```
