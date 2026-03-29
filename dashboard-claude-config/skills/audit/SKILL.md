---
name: audit
user-invocable: true
description: Run all audit agents (security, performance, accessibility) and report findings
args: "[scope] - optional path to scope the audit (e.g., 'app/(app)/keys')"
---

# /audit — Full Codebase Audit

Run all three audit agents and compile a unified report.

## Steps

1. **Run security-reviewer agent** on the codebase (or scoped path).
2. **Run performance-auditor agent** on the codebase (or scoped path).
3. **Run a11y-checker agent** on the codebase (or scoped path).
4. **Compile results** into a single report grouped by severity.

## Output Format

```
# Audit Report — Vintl Dashboard

## Security
[output from security-reviewer]

## Performance
[output from performance-auditor]

## Accessibility
[output from a11y-checker]

## Summary
- Security: N critical, M warnings
- Performance: N issues, M optimizations
- Accessibility: N critical, M warnings
- Overall: PASS / NEEDS ATTENTION
```
