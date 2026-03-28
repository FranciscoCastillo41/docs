---
paths:
  - "app/**/*"
  - "components/**/*"
---

# React / Next.js 15 Standards

## Server Components First

Every component is a Server Component by default. Only add `"use client"` when you need:
- Event handlers (onClick, onChange, onSubmit)
- useState, useReducer, useRef
- useEffect (rare — almost never needed)
- Browser APIs (window, localStorage, IntersectionObserver)

CORRECT:
```typescript
// Server Component — fetches data directly
export default async function DashboardPage() {
  const usage = await getUsage()
  return <UsageChart data={usage} />
}
```

WRONG:
```typescript
"use client"
// Client Component fetching in useEffect — never do this
export default function DashboardPage() {
  const [usage, setUsage] = useState(null)
  useEffect(() => { fetch('/api/usage').then(r => r.json()).then(setUsage) }, [])
}
```

## Data Fetching

- Fetch in Server Components with async/await. Never useEffect.
- Use Server Actions (`"use server"`) for mutations (form submissions, button actions).
- After mutations, call `revalidatePath()` or `revalidateTag()` to refresh data.
- Use `<Suspense fallback={<Skeleton />}>` around async components.

## File Conventions

- `page.tsx` — route page (Server Component unless it needs interactivity)
- `layout.tsx` — shared layout wrapping child routes
- `loading.tsx` — Suspense fallback shown while page loads
- `error.tsx` — error boundary (`"use client"` required)
- `not-found.tsx` — custom 404

## Component Files

- One component per file.
- File name matches component: `api-key-table.tsx` → `export function ApiKeyTable`
- Kebab-case file names, PascalCase component names.
- Colocate: keep component, its types, and its server action in the same directory.

## Patterns

- No prop drilling past 2 levels. Use composition (children) or React Context.
- No inline styles. Tailwind only. Exception: truly dynamic values.
- Dynamic imports for heavy client components: `const Chart = dynamic(() => import('./chart'), { ssr: false })`
- Use `next/image` for all images. Set explicit width/height. Use `priority` for above-fold.
- Use `next/font/google` for fonts. No external font stylesheet links.
- Use `next/link` for internal navigation. Never `<a href>` for internal routes.

## State

- Server state → Server Components (fetch on server, pass down)
- UI state → `useState` / `useReducer` (modals, toggles, form fields)
- URL state → `searchParams` for filters, pagination, tabs
- No Redux, Zustand, or Jotai in this project. Not needed.

## Error Handling

- Every route group gets an `error.tsx` boundary.
- Toast notifications for user action results (success/failure).
- Never swallow errors. Log to console in dev, Sentry in prod.
- Use `useOptimistic` for instant feedback on mutations.
