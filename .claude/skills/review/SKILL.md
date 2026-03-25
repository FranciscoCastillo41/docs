---
name: review
user-invocable: true
description: Review documentation for quality, accuracy, and SEO
---

# /review — Documentation Review

Run a comprehensive review of documentation changes.

## Steps

1. Run `git diff --name-only HEAD` to find changed `.mdx` files. If no changes,
   review all `.mdx` files.

2. Launch the `content-reviewer` agent on the changed files to check:
   - Writing quality (voice, tone, clarity)
   - Code sample quality (runnable, real data, correct format)
   - SEO (title, description, keywords, internal links)
   - Mintlify compatibility (frontmatter, components, headings)

3. Launch the `accuracy-checker` agent to cross-reference documentation
   against `openapi/v1.yaml` for:
   - Endpoint coverage (all spec endpoints documented)
   - Parameter accuracy (names, types, defaults match spec)
   - Response schema accuracy (field names, types, envelope format)

4. Combine results and present a summary:
   - Total files reviewed
   - Issues by category (accuracy, writing, SEO, completeness)
   - Files that pass vs need changes
   - Prioritized list of fixes
