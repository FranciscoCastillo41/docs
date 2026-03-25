---
name: add-endpoint-doc
user-invocable: true
description: Create documentation for a new API endpoint from the OpenAPI spec
args: "<endpoint> - the endpoint path (e.g., 'GET /v1/treasury/yields')"
---

# /add-endpoint-doc — Document an API Endpoint

Create a documentation page for an API endpoint by reading its definition
from the OpenAPI spec.

## Steps

1. **Read the endpoint from the spec**: Find the endpoint in `openapi/v1.yaml`
   and extract its summary, description, parameters, request body, and responses.

2. **Determine the file path**: Map the endpoint to a docs path.
   - `GET /v1/treasury/yields` -> `api-reference/treasury/list-yields.mdx`
   - `GET /v1/series/{id}/observations` -> `api-reference/macro/get-observations.mdx`

3. **Create the MDX file** with:
   - YAML frontmatter: `title`, `description`, `api` (the HTTP method + path)
   - OpenAPI integration tag so Mintlify auto-generates the playground
   - Parameter table with name, type, required/optional, default, description
   - Example request in cURL, Python, TypeScript
   - Example success response using real data
   - Example error responses (400, 401, 429)
   - Links to relevant guide pages

4. **Update mint.json**: Add the page to the API Reference navigation group
   under the correct subgroup (treasury, macro, series, auth).

5. **Cross-link**: Add links from related guide pages to this new endpoint doc.

## Frontmatter Format for API Pages

```yaml
---
title: "List Treasury Yields"
api: "GET /v1/treasury/yields"
description: "Query daily treasury yield rates by maturity and date range"
---
```

The `api` field tells Mintlify to render the interactive API playground
and auto-generate parameter tables from the OpenAPI spec.
