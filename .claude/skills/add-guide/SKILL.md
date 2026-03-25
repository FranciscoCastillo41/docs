---
name: add-guide
user-invocable: true
description: Scaffold a new guide page with correct structure and examples
args: "<topic> - the guide topic (e.g., 'working with CPI data')"
---

# /add-guide — Scaffold a New Guide

Create a new guide page with the correct structure, real API examples, and
proper Mintlify configuration.

## Steps

1. **Determine the file path**: Convert the topic to a slug.
   Example: "working with CPI data" -> `guides/working-with-cpi-data.mdx`

2. **Create the MDX file** with this structure:
   - YAML frontmatter with `title` (< 60 chars) and `description` (< 160 chars with keyword)
   - One-line intro paragraph
   - H2 sections covering the topic
   - CodeGroup with cURL, Python, and TypeScript examples using real API data
   - Real series IDs (GDPC1, CPIAUCSL, UNRATE, etc.) and real dates
   - Financial values as strings in all examples
   - X-API-Key header in all code samples
   - `<Note>`, `<Warning>`, or `<Tip>` callouts where appropriate
   - Links to related API reference pages and other guides

3. **Update mint.json**: Add the new page to the `navigation` array under
   the "Guides" group. Place it in a logical position relative to existing guides.

4. **Verify**: Confirm the file exists and mint.json is valid JSON.

## Template

```mdx
---
title: "<Title Under 60 Chars>"
description: "<Description under 160 chars with target keyword>"
---

<Intro sentence explaining what this guide covers.>

## <First Section>

<Explanation of what the code does.>

<CodeGroup>
```bash cURL
curl -H "X-API-Key: YOUR_API_KEY" \
  "https://api.macrodata.dev/v1/..."
```

```python Python
import requests

resp = requests.get(
    "https://api.macrodata.dev/v1/...",
    headers={"X-API-Key": "YOUR_API_KEY"},
    params={...}
)
data = resp.json()
```

```typescript TypeScript
const resp = await fetch(
  "https://api.macrodata.dev/v1/...?...",
  { headers: { "X-API-Key": "YOUR_API_KEY" } }
);
const data = await resp.json();
```
</CodeGroup>

## Next Steps

- [Related guide](/guides/related-topic)
- [API reference](/api-reference/endpoint)
```
