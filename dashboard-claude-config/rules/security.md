---
paths:
  - "app/api/**/*"
  - "lib/**/*"
  - "middleware.ts"
---

# Security Standards

## Secrets

- NEVER expose server-only keys in client code.
- Server-only: `SUPABASE_SERVICE_ROLE_KEY`, `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, `INTERNAL_API_TOKEN`
- Client-safe (prefixed `NEXT_PUBLIC_`): `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `STRIPE_PUBLISHABLE_KEY`, `ROOT_DOMAIN`
- NEVER log, return in responses, or store raw API keys. Only store SHA-256 hashes.
- NEVER commit `.env` files. They are in `.gitignore`.

CORRECT:
```typescript
// Server-only — no NEXT_PUBLIC_ prefix
const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!)
```

WRONG:
```typescript
// This exposes the key to the browser
const stripe = new Stripe(process.env.NEXT_PUBLIC_STRIPE_SECRET_KEY!)
```

## Input Validation

- Validate ALL external input with Zod: request bodies, query params, form data, webhook payloads.
- Validate on the server, never trust client-side validation alone.
- Return generic error messages to the client. Log details server-side.

## Authentication

- Always call `supabase.auth.getUser()` in Server Components and API routes. Never trust the session cookie alone.
- Middleware refreshes sessions but does NOT replace per-route auth checks.
- API route handlers must verify the user before any action.

CORRECT:
```typescript
export async function POST(request: Request) {
  const supabase = await createClient()
  const { data: { user }, error } = await supabase.auth.getUser()
  if (!user) return new Response('Unauthorized', { status: 401 })
  // proceed
}
```

## Stripe Webhooks

- ALWAYS verify webhook signatures with `stripe.webhooks.constructEvent()`.
- Use the raw request body (not parsed JSON) for signature verification.
- Use `SUPABASE_SERVICE_ROLE_KEY` (not anon key) for webhook database writes — webhooks run without a user session.

## Headers

- CSP, X-Frame-Options, HSTS set in `next.config.ts`.
- Allow Stripe JS (`js.stripe.com`) in CSP script-src.
- DENY framing to prevent clickjacking.

## Rate Limiting

- Rate limit auth endpoints (login, signup, forgot-password) with `@upstash/ratelimit` or similar.
- Rate limit API key creation to prevent abuse.
