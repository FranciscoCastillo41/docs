---
name: add-api-route
user-invocable: true
description: Scaffold a Next.js API route handler with auth, validation, and error handling
args: "<path> <methods> - e.g., 'keys GET,POST' or 'billing/portal POST'"
---

# /add-api-route — Scaffold an API Route Handler

Create a new API route handler with authentication, Zod validation, and proper error handling.

## Steps

1. **Create the route file**: `app/api/<path>/route.ts`

2. **Add auth check**: Every route must verify the user with Supabase.

3. **Add Zod validation**: For POST/PATCH/PUT request bodies.

4. **Add error handling**: Try/catch with proper HTTP status codes.

5. **Verify**: Run `pnpm tsc --noEmit`.

## Template

```typescript
// app/api/<path>/route.ts
import { NextResponse } from 'next/server'
import { z } from 'zod'
import { createClient } from '@/lib/supabase/server'

const createSchema = z.object({
  name: z.string().min(1).max(100),
})

export async function GET() {
  const supabase = await createClient()
  const { data: { user } } = await supabase.auth.getUser()
  if (!user) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }

  try {
    // fetch data
    return NextResponse.json({ data })
  } catch (error) {
    console.error('[API] GET /api/<path> error:', error)
    return NextResponse.json({ error: 'Internal server error' }, { status: 500 })
  }
}

export async function POST(request: Request) {
  const supabase = await createClient()
  const { data: { user } } = await supabase.auth.getUser()
  if (!user) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }

  try {
    const body = createSchema.parse(await request.json())
    // handle request
    return NextResponse.json({ data }, { status: 201 })
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json({ error: error.errors }, { status: 400 })
    }
    console.error('[API] POST /api/<path> error:', error)
    return NextResponse.json({ error: 'Internal server error' }, { status: 500 })
  }
}
```

## Rules

- Every handler checks `supabase.auth.getUser()` first
- POST/PATCH/PUT bodies validated with Zod
- Errors return proper HTTP status codes
- Console.error with route prefix for debugging
- If proxying to Go API, use `INTERNAL_API_TOKEN` from env (server-only)
- Never return raw API keys in responses — only prefixes
