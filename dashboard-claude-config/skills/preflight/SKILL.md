---
name: preflight
user-invocable: true
description: Run all checks before creating a PR — lint, types, tests, build
args: ""
---

# /preflight — Pre-PR Checks

Run the full quality gate before pushing or creating a PR.

## Steps

Run these sequentially. Stop on first failure.

1. **Lint**: `pnpm lint`
   - Must pass with zero warnings
   - Fix any issues found

2. **Type check**: `pnpm tsc --noEmit`
   - Must pass with zero errors
   - Fix any type issues

3. **Unit tests**: `pnpm test`
   - All tests must pass
   - Report coverage summary

4. **Build**: `pnpm build`
   - Must complete without errors
   - Report build size summary

5. **Report**:
```
## Preflight Report
- Lint: PASS (0 warnings)
- Types: PASS (0 errors)
- Tests: PASS (N passed, M skipped)
- Build: PASS (First Load JS: XXkB)
- Verdict: READY TO PUSH
```

If any step fails, stop and report what needs to be fixed before retrying.
