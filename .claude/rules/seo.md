---
paths:
  - "**/*.mdx"
---

# SEO Standards for API Documentation

Based on Redocly's API documentation SEO research and Google's developer
docs guidelines. API docs rank well because they answer specific queries
developers actually search for.

## Page Metadata

- Every page `title` must be unique and under 60 characters
- Every page `description` must be unique, under 160 characters, and include a target keyword
- H1 is auto-generated from `title` in frontmatter — never add a manual `# Heading`
- `sidebarTitle` can differ from `title` for navigation clarity

CORRECT:
```yaml
---
title: "Point-in-Time Queries"
description: "Query what economic data was known at any historical date with the as_of parameter"
sidebarTitle: "Point-in-Time (as_of)"
---
```

WRONG:
```yaml
---
title: "Point-in-Time Queries — Vintl API Documentation for Historical Vintage Data Access"
description: "docs"
---
```

## Target Keywords

Use these naturally in H2 headings and body text:
- "GDP API" / "GDP data API"
- "macro economic data API"
- "historical economic data API"
- "FRED API alternative"
- "treasury yield curve API"
- "CPI data API"
- "point in time economic data"
- "vintage macro data"
- "Python economic data API"
- "backtest economic data"

CORRECT: `## Query historical GDP data` (natural, includes "historical" + "GDP data")
WRONG: `## GDP API FRED Alternative Historical Macro Economic Data` (keyword stuffing)

## URL Structure

- Slugs are lowercase-hyphenated: `point-in-time`, not `pointInTime` or `Point_In_Time`
- URL path matches file path: `guides/point-in-time.mdx` -> `/guides/point-in-time`
- Keep URLs short and descriptive

## Internal Linking

- Every guide must link to at least one API reference page
- Every API reference page should link to relevant guides
- Use descriptive link text: `[point-in-time queries](/guides/point-in-time)`
- Never use "click here" or "this page" as link text

CORRECT: "Learn more about [point-in-time queries](/guides/point-in-time)."
WRONG: "For more information, [click here](/guides/point-in-time)."

## Search Intent Mapping

Each guide should answer a specific search query:
- `guides/point-in-time.mdx` -> "point in time economic data", "vintage macro data"
- `guides/yield-curve.mdx` -> "treasury yield curve API"
- `guides/python-quickstart.mdx` -> "Python economic data API"
- `quickstart.mdx` -> "macro economic data API"
- `introduction.mdx` -> "FRED API alternative", "GDP API"
