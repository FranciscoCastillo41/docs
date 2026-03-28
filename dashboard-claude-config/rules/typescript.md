---
paths:
  - "**/*.ts"
  - "**/*.tsx"
---

# TypeScript Standards

Strict mode. No exceptions.

## Types

- `any` is banned. Use `unknown` and narrow with type guards.
- No `as` type assertions unless interfacing with an untyped library. Add a comment explaining why.
- Infer return types for internal functions. Annotate exported functions.
- Use `satisfies` for const objects: `const x = { ... } satisfies Record<string, Plan>`.
- Prefer `interface` for object shapes, `type` for unions and intersections.

CORRECT:
```typescript
export function getUser(id: string): Promise<User | null> {
  // ...
}
const plans = { free: { limit: 500 } } satisfies Record<string, Plan>
```

WRONG:
```typescript
export function getUser(id: any) {
  return data as User
}
```

## Imports

- No barrel exports (`index.ts` re-exporting everything). Import directly from the source file.
- Group imports: React/Next → external packages → internal `@/lib` → internal `@/components` → relative.
- Use `@/` path alias for all non-relative imports.

CORRECT:
```typescript
import { Suspense } from 'react'
import { z } from 'zod'
import { createClient } from '@/lib/supabase/server'
import { ApiKeyTable } from '@/components/dashboard/api-key-table'
```

WRONG:
```typescript
import { ApiKeyTable } from '@/components'
import { createClient } from '../../../../lib/supabase/server'
```

## Validation

- Zod for every external boundary: form inputs, API route bodies, webhook payloads, URL params.
- Define the schema next to where it's used, not in a central `schemas/` folder.
- Parse, don't validate: `schema.parse(data)` throws, `schema.safeParse(data)` returns a result.

```typescript
const createKeySchema = z.object({
  name: z.string().min(1).max(100),
})

export async function POST(request: Request) {
  const body = createKeySchema.parse(await request.json())
  // body is typed and validated
}
```
