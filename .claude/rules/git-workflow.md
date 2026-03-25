---
paths:
  - "**/*"
---

# Git Workflow Standards

Trunk-based development (GitHub Flow). Based on GitHub's own workflow documentation
and Conventional Commits specification (conventionalcommits.org).

## Branching

- `main` is always deployable. Never commit directly to `main`.
- Feature branches off main, named descriptively:
  `feature/quickstart-page`, `feature/point-in-time-guide`
- One branch per page or logical group of changes.
- Short-lived: hours, not weeks. Merge and delete.

CORRECT: `feature/quickstart-page`, `feature/add-yield-curve-guide`
WRONG: `dev`, `release/v2`, `hotfix/fix-thing`, `francisco/stuff`

## Commits

Conventional Commits format. Message describes WHY, not WHAT.

CORRECT:
```
docs: add quickstart page with curl/python/typescript examples
docs: add point-in-time guide — the killer feature page
fix: correct GDP vintage dates in code samples
chore: update openapi spec from market-data-api
style: fix MDX formatting in authentication page
```

WRONG:
```
update files
WIP
fix stuff
added new page
```

Rules:
- One logical change per commit
- Never commit broken docs.json (navigation must match existing .mdx files)
- Always end with: `Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>`

## PR Process

1. Create feature branch from main
2. Make changes, commit
3. Push branch, create PR with clear title
4. Mintlify preview deployment is automatic on PR
5. Verify preview looks correct
6. Squash merge into main, delete branch
7. Mintlify auto-deploys to production on main push

## Pre-PR Quality Checks

Before creating a PR, verify all of these:
- [ ] docs.json is valid JSON
- [ ] Every page in navigation has a corresponding .mdx file
- [ ] All code samples use real series IDs (GDPC1, CPIAUCSL, UNRATE)
- [ ] All code samples use real dates that return data
- [ ] Financial values are strings: `"4.39"` not `4.39`
- [ ] All pages have frontmatter with `title` and `description`
- [ ] No broken internal links
- [ ] No H1 headings (title generates H1)
- [ ] No H4+ headings (only H2 and H3)
- [ ] CodeGroup language order: cURL -> Python -> TypeScript
- [ ] No use of "realtime" — say "point-in-time" or "vintage"
