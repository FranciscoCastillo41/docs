---
paths:
  - "**/*.mdx"
---

# MDX Content Standards

Standards grounded in Stripe's docs writing guide, Google Developer Documentation
Style Guide, and Microsoft Writing Style Guide.

## Voice and Tone (Stripe docs writing guide)

- Second person: "you" and "your", never "the user" or "one"
- Present tense: "returns" not "will return", "sends" not "will send"
- Active voice: "Set the header" not "The header should be set"
- Direct: "This endpoint requires auth" not "It should be noted that..."
- No filler: cut "simply", "just", "easily", "basically", "obviously"

CORRECT: "Add the `X-API-Key` header to every request."
WRONG: "You will need to simply add the X-API-Key header to your requests."

## Clarity (Google Developer Documentation Style Guide)

- One idea per sentence. If a sentence has "and" or "but", split it.
- Define acronyms on first use: "Consumer Price Index (CPI)"
- Sentence case for headings: "Get yield curve data" not "Get Yield Curve Data"
- Use "select" not "click on", "enter" not "type in"

CORRECT: "Pass the `as_of` parameter to query historical data."
WRONG: "In order to be able to query data from a historical point in time, you can use the as_of parameter which will allow you to specify the date."

## Code Samples

- Every sample must be copy-pasteable and produce a real response
- Use real series IDs: GDPC1, CPIAUCSL, UNRATE, PAYEMS, FEDFUNDS, INDPRO
- Never use placeholder IDs like "EXAMPLE_SERIES" or "YOUR_SERIES"
- Use real dates that produce interesting data:
  - 2023-10-26 for GDP advance estimate
  - 2023-07-01 for Q3 2023 observation date
- Financial values are ALWAYS strings: `"4.39"` not `4.39`
- Always show the X-API-Key header: `"X-API-Key": "YOUR_API_KEY"`
- Language order in CodeGroup: cURL first, Python second, TypeScript third

CORRECT:
```json
{"rate": "4.39", "maturity": "10Y"}
```

WRONG:
```json
{"rate": 4.39, "maturity": "10Y"}
```

## Page Structure

1. YAML frontmatter with `title` (< 60 chars) and `description` (< 160 chars)
2. One-line intro sentence immediately after frontmatter
3. H2 (`##`) for major sections
4. H3 (`###`) for subsections within an H2
5. NEVER use H4 (`####`) or deeper — restructure instead
6. Code block immediately after the sentence explaining what it does
7. Tables for parameter references, not prose lists
8. Callouts for important context: `<Note>`, `<Warning>`, `<Tip>`

CORRECT:
```
## Authentication
### API Key Header
```

WRONG:
```
## Authentication
### API Key Header
#### Key Format
##### Prefix Details
```

## Links and References

- Internal links use relative paths: `[point-in-time](/guides/point-in-time)`
- Never use full URLs for internal links: not `https://docs.macrodata.dev/guides/...`
- Cross-link between guides and API reference pages
- Every guide should link to the relevant API reference endpoint

## Terminology

- Never say "realtime" — that is FRED's confusing terminology
- Say "point-in-time" or "vintage" instead
- Dates in prose: YYYY-MM-DD (`2023-10-26`)
- Dates in JSON examples: ISO 8601 (`"2023-10-26T00:00:00Z"`)
- Example responses must match the actual API envelope format exactly

CORRECT: "Use point-in-time queries to see what was known on 2023-10-26."
WRONG: "Use realtime queries to access the real-time data snapshot."

## File Organization

- One concept per page. If a page exceeds 300 lines, split it.
- Snippets go in `/snippets/` — import with `<Snippet file="snippets/name.mdx" />`
- Images go in `/images/` with descriptive filenames: `gdp-revision-timeline.png`

## Content Boundaries

- No marketing language in API reference pages
- Marketing language is acceptable in `introduction.mdx` and `guides/` pages
- No pricing details in docs — link to the dashboard instead
- No internal implementation details (Redis caching, database schemas)
