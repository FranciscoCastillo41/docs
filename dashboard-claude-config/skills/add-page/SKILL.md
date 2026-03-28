---
name: add-page
user-invocable: true
description: Scaffold a new page with correct Next.js 15 App Router conventions
args: "<route-group> <path> - e.g., 'app dashboard/usage' or 'marketing blog'"
---

# /add-page — Scaffold a New Page

Create a new page following Next.js 15 App Router conventions with proper layout, loading, and error handling.

## Steps

1. **Determine the route group and path** from the args:
   - `app` → `app/(app)/<path>/page.tsx` (authenticated, sidebar layout)
   - `marketing` → `app/(marketing)/<path>/page.tsx` (public, marketing layout)
   - `auth` → `app/(auth)/<path>/page.tsx` (centered card layout)

2. **Create the page file** (`page.tsx`):
   - Server Component by default (no `"use client"`)
   - Fetch data with async/await if needed
   - Export metadata for SEO
   - Import from `@/` path alias

3. **Create `loading.tsx`** in the same directory:
   - Skeleton UI matching the page layout
   - Use shadcn `Skeleton` component

4. **Create `error.tsx`** if the page fetches data:
   - Must be `"use client"`
   - Show error message with retry button
   - Log error to console

5. **Verify**: Run `pnpm tsc --noEmit` to check types.

## Template

```typescript
// app/(app)/<path>/page.tsx
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: '<Page Title> | MacroData',
  description: '<Page description>',
}

export default async function PageName() {
  return (
    <div className="space-y-6">
      <h1 className="text-2xl font-bold">Page Title</h1>
      {/* content */}
    </div>
  )
}
```

```typescript
// app/(app)/<path>/loading.tsx
import { Skeleton } from '@/components/ui/skeleton'

export default function Loading() {
  return (
    <div className="space-y-6">
      <Skeleton className="h-8 w-48" />
      <Skeleton className="h-64 w-full" />
    </div>
  )
}
```
