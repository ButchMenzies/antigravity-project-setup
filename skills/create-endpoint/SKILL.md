---
name: Create Endpoint
description: API route or server action with validation, auth, and error handling
triggers: [new endpoint, API route, server action, new route, REST endpoint]
track_types: [feature, chore]
mcps:
  - name: supabase
    usage: query data, check RLS, verify auth
    required: false
---

# Create Endpoint

## When to Use

When the track involves creating a new API endpoint (Next.js API route, server action, or Express route). Use alongside **database-change** if the endpoint requires schema modifications.

## Plan Checklist

### Design

- [ ] Define the endpoint contract: method, path, request body, response shape
- [ ] Choose the right pattern for the framework:
  - Next.js App Router: server actions for mutations, route handlers for external APIs
  - Express: router + controller pattern
- [ ] Follow existing project conventions — don't mix patterns

### Input Validation

- [ ] Validate all input with zod (or project's validation library)
- [ ] Validate path parameters, query parameters, and request body
- [ ] Return 400 with clear error messages for invalid input — never pass unvalidated data to the database

### Authentication & Authorization

- [ ] Add auth check — who can call this endpoint?
- [ ] Verify the user has permission for the specific resource (not just "is logged in")
- [ ] Use middleware or RLS — don't repeat auth checks in every handler

### Error Handling

- [ ] Return typed error responses with consistent format
- [ ] Don't expose internal errors to the client (no raw stack traces)
- [ ] Handle common cases: not found (404), unauthorized (401), forbidden (403), validation error (400)
- [ ] Log errors server-side for debugging

### Response

- [ ] Return consistent response shapes — don't mix `{ data }` and `{ result }` across endpoints
- [ ] Include appropriate HTTP status codes
- [ ] Set proper headers (Content-Type, caching where appropriate)

### Verification

- [ ] Endpoint responds correctly with valid input
- [ ] Returns appropriate error for invalid input
- [ ] Auth check works — unauthenticated requests are rejected
- [ ] TypeScript types match the actual response shape

## Common Mistakes

1. **No input validation** — trusting client-side data leads to security issues
2. **Inconsistent response format** — each endpoint returns data differently, making the client code messy
3. **Exposing internal errors** — returning raw database errors or stack traces to the client
4. **Missing auth checks** — endpoint works but anyone can call it
5. **Mixing patterns** — some endpoints use server actions, others use route handlers, with no clear reasoning
