---
name: create-feature
description: >-
  Builds full vertical slices from database to UI with nothing forgotten.
  Use when implementing new user-facing features, adding capabilities, or
  building functionality that spans multiple layers.
source: antigravity
---

# Create Feature

## When to Use

When the track involves building new user-facing functionality that touches multiple layers (database, API, UI). Skip for pure config changes or refactors.

## Plan Checklist

Incorporate these items into the implementation plan phases. Skip any that don't apply to the specific feature.

### Pre-build

- [ ] Scan `components/` (or equivalent) for existing components that can be reused or extended — do NOT create new components when similar ones exist
- [ ] Identify all layers this feature touches (database, API, UI, auth)
- [ ] If database changes are needed, read the **database-change** skill

### Database Layer (if applicable)

- [ ] Write migration for schema changes
- [ ] Add or verify RLS policies for any new/modified tables
- [ ] Regenerate TypeScript types from updated schema
- [ ] If Supabase MCP available: run `get_advisors` security check after migration

### API / Server Layer

- [ ] Create server actions, API routes, or edge functions as appropriate
- [ ] Add input validation (zod or equivalent)
- [ ] Add typed error responses — don't return raw errors to the client
- [ ] Add auth checks where needed (middleware, RLS, or manual)

### UI Layer

- [ ] Build using existing shared components — extend with props/variants rather than duplicating
- [ ] Include **loading state** (`loading.tsx`, Suspense boundary, or skeleton)
- [ ] Include **error state** (`error.tsx`, error boundary, or inline)
- [ ] Include **empty state** (no data yet UI)
- [ ] Verify responsive layout at mobile, tablet, and desktop widths
- [ ] If `.agent/ux/` exists, check `design-direction.md` and `patterns.md` for consistency

### Verification

- [ ] Feature works end-to-end (create, read, update, delete as applicable)
- [ ] RLS policies tested — correct user has access, others do not
- [ ] TypeScript compiles with no type errors
- [ ] No console errors or warnings in browser

## Common Mistakes

1. **Building UI before confirming the data model** — always start with the schema, then API, then UI
2. **Forgetting loading/error/empty states** — every data-dependent view needs all three
3. **Creating new components when similar ones exist** — always scan first, extend with variants
4. **Skipping RLS on new Supabase tables** — every table needs a policy, even if it's just "authenticated users"
5. **Not regenerating TypeScript types** — schema changes without type regen cause runtime crashes that TypeScript should catch
6. **Mixing server and client concerns** — don't use `useState` in server components, don't do database calls in client components
