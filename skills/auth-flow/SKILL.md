---
name: Auth Flow
description: Authentication and authorization — protected routes, roles, middleware
triggers: [auth, login, signup, protected route, authorization, role, permission]
track_types: [feature, chore]
mcps:
  - name: supabase
    usage: auth configuration, RLS policies by user, session management
    required: false
---

# Auth Flow

## When to Use

When the track involves adding authentication, protecting routes, implementing role-based access, or modifying authorization logic. Also use when adding features that require user-specific data access.

## Plan Checklist

### Authentication Setup (if not already configured)

- [ ] Auth provider configured (Supabase Auth, NextAuth, etc.)
- [ ] Login/signup pages or components created
- [ ] Session handling: cookies, JWT, or session storage as appropriate
- [ ] Auth callback route handles OAuth redirects (if using social login)

### Protected Routes

- [ ] Middleware checks auth status before rendering (Next.js `middleware.ts` or route guards)
- [ ] Unauthenticated users redirected to login — not shown a blank page
- [ ] Post-login redirect returns user to the page they originally requested
- [ ] Public routes explicitly defined — don't accidentally protect everything

### Authorization (Role-Based)

- [ ] User roles stored in database (not just JWT claims that can go stale)
- [ ] RLS policies reference the authenticated user: `auth.uid()` or `auth.jwt()`
- [ ] Server-side authorization checks — never trust client-side role checks alone
- [ ] UI hides elements the user can't access, but server enforces the real check

### Session Management

- [ ] Session refresh handles token expiration gracefully
- [ ] Logout clears all auth state (cookies, local storage, server session)
- [ ] Multiple tabs stay in sync after login/logout

### Verification

- [ ] Unauthenticated user cannot access protected routes
- [ ] Authenticated user can access permitted resources
- [ ] User A cannot access user B's data (test as different users)
- [ ] Login → protected page → logout → cannot access flow works end-to-end

## Common Mistakes

1. **Client-side only auth checks** — hiding UI elements but not enforcing on the server
2. **No middleware** — every page does its own auth check (or forgets to)
3. **Stale JWT claims** — role changes don't take effect until token refresh
4. **No post-login redirect** — user always lands on homepage after login, losing context
5. **Forgetting RLS** — auth works at the API level but database is wide open
