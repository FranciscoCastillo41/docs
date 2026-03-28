---
paths:
  - "**/*.tsx"
  - "tailwind.config.ts"
---

# Tailwind CSS Standards

## Design Tokens

Use the shared design tokens from `tailwind.config.ts`. Never hardcode colors.

```
Primary: #F59E0B (amber) — matches docs.macrodata.dev
Light:   #FBBF24
Dark:    #D97706
Font:    Inter
```

CORRECT: `bg-primary`, `text-primary-dark`, `border-border`
WRONG: `bg-[#F59E0B]`, `text-[#D97706]`, `border-[#262626]`

## Dark Mode

- Dark mode is the default (`darkMode: 'class'`).
- Every component MUST look correct in both dark and light mode.
- Test by toggling the theme. If it looks broken, fix it before committing.

## Responsiveness

- Mobile-first: default styles are mobile (375px).
- Add `md:` (768px) and `lg:` (1024px) breakpoints for larger screens.
- Test every page at 375px, 768px, and 1280px.
- The dashboard sidebar collapses to a hamburger on mobile.

## Class Discipline

- No magic numbers. Use Tailwind spacing: `p-4` not `p-[17px]`.
- Max 6 utility classes per element before extracting a component.
- No `@apply` in CSS files. If you need reusable styles, make a component.
- No inline `style` attributes unless the value is truly dynamic.

CORRECT:
```tsx
<div className="rounded-lg border border-border bg-background-card p-6">
```

WRONG:
```tsx
<div className="rounded-[12px] border border-[#262626] bg-[#141414] p-[24px]">
```

## shadcn/ui

- Use shadcn/ui for all primitives: Button, Card, Dialog, Toast, Input, etc.
- Install with `npx shadcn@latest add <component>`.
- Customize via CSS variables in `globals.css`, not by editing component files.
- If you modify a shadcn component, add a comment explaining why.
