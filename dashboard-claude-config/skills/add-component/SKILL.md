---
name: add-component
user-invocable: true
description: Scaffold a new React component with proper types, tests, and conventions
args: "<directory> <name> - e.g., 'dashboard api-key-table' or 'marketing pricing-card'"
---

# /add-component — Scaffold a New Component

Create a new component following project conventions with TypeScript types.

## Steps

1. **Determine the directory** from the first arg:
   - `dashboard` → `components/dashboard/<name>.tsx`
   - `marketing` → `components/marketing/<name>.tsx`
   - `shared` → `components/shared/<name>.tsx`
   - `ui` → use `npx shadcn@latest add` instead, don't create manually

2. **Determine if it needs `"use client"`**:
   - Does it use onClick, onChange, onSubmit? → yes
   - Does it use useState, useRef, useEffect? → yes
   - Is it purely presentational (takes props, renders JSX)? → no, keep as Server Component

3. **Create the component file**:
   - One component per file
   - Export named function (not default export for non-page components)
   - Define props interface in the same file
   - Use Tailwind for all styling
   - Use shadcn/ui primitives where possible

4. **Create a test file** if the component has logic:
   - `components/dashboard/<name>.test.tsx`
   - Test with Vitest + React Testing Library

5. **Verify**: Run `pnpm tsc --noEmit`.

## Template

```typescript
// components/dashboard/<name>.tsx
interface ApiKeyTableProps {
  keys: Array<{
    id: string
    prefix: string
    name: string
    createdAt: string
    lastUsedAt: string | null
  }>
  onRevoke: (id: string) => Promise<void>
}

export function ApiKeyTable({ keys, onRevoke }: ApiKeyTableProps) {
  return (
    <div className="rounded-lg border border-border">
      {/* component content */}
    </div>
  )
}
```
