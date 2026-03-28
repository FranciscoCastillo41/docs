---
paths:
  - "**/*"
---

# Git Workflow Standards

Trunk-based development (GitHub Flow).

## Branching

- `main` is always deployable. Never commit directly to `main`.
- Feature branches off main, named descriptively:
  `feature/landing-page`, `feature/key-management`, `feature/stripe-billing`
- One branch per feature or logical group of changes.
- Short-lived: hours, not weeks. Merge and delete.

CORRECT: `feature/landing-page`, `feature/api-key-crud`, `fix/stripe-webhook-verification`
WRONG: `dev`, `release/v2`, `francisco/stuff`, `WIP`

## Commits

Conventional Commits format. Message describes WHY, not WHAT.

CORRECT:
```
feat: add API key creation with copy-once modal
fix: handle expired Supabase sessions in middleware
ui: update pricing card hover states for dark mode
chore: upgrade @supabase/ssr to 0.6.x
test: add E2E tests for signup flow
```

WRONG:
```
update files
WIP
fix stuff
changes
```

Rules:
- One logical change per commit
- Never commit broken builds
- Always end with: `Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>`

## Pre-commit Checks (Husky)

On commit: `pnpm lint-staged` (ESLint + Prettier on staged files)
On push: `pnpm tsc --noEmit`

## Pre-PR Quality Checks

Before creating a PR, verify:
- [ ] `pnpm build` succeeds
- [ ] `pnpm lint` passes with zero warnings
- [ ] `pnpm tsc --noEmit` passes
- [ ] `pnpm test` passes
- [ ] No `console.log` left in code (use proper logging)
- [ ] No hardcoded secrets or API keys
- [ ] Dark mode works on all changed components
- [ ] Mobile responsive on all changed pages (375px)
- [ ] Accessibility: proper labels, focus states, keyboard navigation

## PR Process

1. Create feature branch from main
2. Make changes, commit with conventional commits
3. Push branch, create PR with clear title
4. Vercel preview deployment is automatic on PR
5. Verify preview works correctly
6. Squash merge into main, delete branch
7. Vercel auto-deploys to production on main merge
