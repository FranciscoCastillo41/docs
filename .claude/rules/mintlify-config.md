---
paths:
  - "docs.json"
---

# Mintlify Configuration Standards

docs.json is the central configuration for the entire docs site.
Breaking this file breaks the deployment. (Migrated from legacy mint.json.)

## Required Fields

- `$schema` must be `"https://mintlify.com/docs.json"`
- `name` must be `"Vintl API"`
- `api.baseUrl` must be `"https://api.vintl.io"`
- `api.auth.method` must be `"key"`
- `api.auth.name` must be `"X-API-Key"`
- API Reference tab must have `"openapi": "openapi/v1.yaml"` for auto-generation

## Navigation Structure

docs.json uses a `navigation.tabs` array. Each tab has `groups`, each group has `pages`.
The API Reference tab uses `openapi` instead of `groups` — Mintlify auto-generates pages.

```json
{
  "navigation": {
    "tabs": [
      { "tab": "Documentation", "groups": [...] },
      { "tab": "API Reference", "openapi": "openapi/v1.yaml" }
    ]
  }
}
```

## Navigation Rules

- Every page in `groups` must have a corresponding `.mdx` file
- Never add a page to navigation without creating the `.mdx` file first
- Group order: Getting Started -> Core Concepts -> Guides -> SDKs -> Changelog
- API Reference is auto-generated from OpenAPI spec — no manual `.mdx` files needed
- Page paths omit the `.mdx` extension
- Navigation must scale: new product areas slot in as new groups
- Never change existing page URLs — this breaks external links and SEO

CORRECT:
```json
{"group": "Getting Started", "pages": ["introduction", "quickstart", "authentication"]}
```

WRONG:
```json
{"group": "Getting Started", "pages": ["introduction", "quickstart.mdx", "new-page-that-doesnt-exist"]}
```

## Validation Checklist

Before modifying docs.json:
1. Verify every page path in navigation has a matching `.mdx` file
2. Color values are valid 6-digit hex: `"#0F172A"` not `"#0F172A00"` or `"blue"`
3. All URLs in `topbar` and `footerSocials` are complete with `https://`
4. The `openapi` path on the API Reference tab matches the actual file

## Do Not Change Without Reason

- `api.baseUrl` — this is the production API URL
- `api.auth` — this matches the actual API auth scheme
- `$schema` — Mintlify uses this for validation
- `openapi` on the API Reference tab — auto-generates all endpoint pages
