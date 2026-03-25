# MacroData API — Developer Documentation

## What This Is

Mintlify Pro documentation site for MacroData API (`docs.macrodata.dev`).
MDX content pushed to GitHub, Mintlify auto-deploys on merge to `main`.

## Architecture

```
MDX files + docs.json → GitHub → Mintlify → docs.macrodata.dev
OpenAPI spec (v1.yaml) → Mintlify → auto-generated API reference
```

## What This Repo Contains

- MDX documentation pages (guides, concepts, changelog)
- `docs.json` — central Mintlify configuration (navigation, theme, API settings)
- `openapi/v1.yaml` — OpenAPI 3.1 spec copied from `market-data-api` repo
- Static assets (logos, favicon) in root directories
- Reusable MDX snippets in `snippets/`

## The Ecosystem (Multiple Repos)

This docs site is one piece of a larger platform:

| Repo | Status | Purpose |
|---|---|---|
| `market-data-api` | Built (v1) | Go REST API — treasury, macro, point-in-time |
| `market-data-pipeline` | Built | Python pipeline — ingests FRED, Treasury.gov into TimescaleDB |
| `market-data-docs` | **This repo** | Mintlify docs at docs.macrodata.dev |
| `market-data-dashboard` | Not built | Next.js + Supabase — signup, API keys, billing |
| `market-data-sdk` | Not built | Python SDK — `pip install macrodata`, returns polars DataFrames |
| `market-data-mcp` | Not built | MCP server for Claude/GPT function calling |
| `macrodata.dev` | Not built | Marketing landing page |

## What This Repo Does NOT Contain

- No backend code. No API logic. No database schemas.
- No SDK code (that will be `market-data-sdk`).
- No dashboard/signup (that will be `market-data-dashboard`).
- No landing page (that will be `macrodata.dev` root).

## OpenAPI Spec Sync

The source of truth for the API spec is `market-data-api/api/openapi/v1.yaml`.
To sync: copy that file into `openapi/v1.yaml` in this repo.
Do NOT edit `openapi/v1.yaml` directly here — changes go to the API repo first.

## Content Structure

| Path | Purpose |
|---|---|
| `introduction.mdx` | Hero page with product pitch |
| `quickstart.mdx` | 2 minutes to first API call |
| `authentication.mdx` | API key usage and management |
| `rate-limits.mdx` | Plans, headers, 429 handling |
| `errors.mdx` | RFC 9457 error reference |
| `pagination.mdx` | Cursor-based pagination |
| `guides/` | In-depth how-to guides |
| `api-reference/` | Auto-generated from OpenAPI spec |
| `changelog.mdx` | Release notes |
| `snippets/` | Reusable MDX components |

## Commands

```bash
# Install Mintlify CLI (once)
npm i -g mint

# Local preview with hot reload (http://localhost:3000)
mint dev

# Check broken links
mint broken-links
```

## Scalability

The docs must accommodate future API expansion without restructuring:
- **v1.1**: Webhooks, bulk export, 100+ FRED series, calculated indicators
- **v1.2**: International data (ECB, BOE, BOJ), equity indices, GraphQL
- **v2.0**: Streaming WebSocket, custom series, team workspaces

Design navigation so new product areas (International, Equities, Streaming) can be added as top-level sections without breaking existing URLs or sidebar structure. The `sdks/` directory exists as a placeholder for future Python, TypeScript, and R SDK docs.

## Git Workflow

Trunk-based development (GitHub Flow):
- `main` is always deployable. Never commit directly to `main`.
- Feature branches off main: `feature/quickstart-page`, `feature/point-in-time-guide`
- Short-lived branches (hours, not weeks). One branch per page or logical group.
- PR into main with descriptive title. Squash merge. Delete branch after merge.

### Commit Format

Conventional Commits. Message describes the WHY, not the WHAT.

```
docs: add quickstart page with curl/python/typescript examples
docs: add point-in-time guide — the killer feature page
fix: correct GDP vintage dates in code samples
chore: update openapi spec from market-data-api
style: fix MDX formatting in authentication page
```

One logical change per commit. Never commit broken docs.json.

### PR Checklist

Before creating a PR, verify:
1. docs.json is valid JSON and every navigation page has a `.mdx` file
2. Code samples use real series IDs, real dates, string financial values
3. All pages have frontmatter with `title` and `description`
4. No broken internal links
5. No H1 headings (title generates H1), no H4+ headings
6. CodeGroup language order: cURL -> Python -> TypeScript

## Critical Rules

1. **docs.json is the central config** — every page in navigation must have a corresponding `.mdx` file. Never break this mapping.
2. **Financial values are strings** — always `"4.39"`, never `4.39`. This is a financial data product — precision is non-negotiable.
3. **Real data in examples** — use actual series IDs (GDPC1, CPIAUCSL, UNRATE) and real dates (2023-10-26 for GDP advance estimate).
4. **Code samples must be runnable** — copy-paste into a terminal and get a real response against the real API.
5. **The killer feature is `?as_of=DATE`** — point-in-time queries. The guide at `guides/point-in-time.mdx` is the most important page after quickstart.
6. **URLs are permanent** — once a page is live, its URL never changes. If you must move a page, add a redirect in docs.json.
7. **Never say "realtime"** — that's FRED's confusing terminology. We say "point-in-time" or "vintage".
8. **Dates**: YYYY-MM-DD in prose, ISO 8601 in JSON examples.

## File Organization

- Keep `docs-bootstrap.md` in repo root as reference
- Never modify `openapi/v1.yaml` directly — it comes from `market-data-api`
- Snippets in `/snippets/`, imported with `<Snippet file="snippets/name.mdx" />`
- Images in `/images/` with descriptive filenames: `gdp-revision-timeline.png`, not `img1.png`
- One concept per page. If a page exceeds 300 lines, split it.

## Domains

- `docs.macrodata.dev` — this site (CNAME to Mintlify)
- `api.macrodata.dev` — the API (separate repo)
- `app.macrodata.dev` — dashboard (separate repo, not built yet)
- `macrodata.dev` — landing page (separate, not built yet)
