---
name: supabase-auth-vite
description: Supabase authentication patterns for Vite + Express apps. Covers AuthContext, session flow, protected routes, and user management.
---

# Supabase Auth for Vite + Express

Authentication patterns specific to our stack (NOT Next.js). Covers session management, the AuthContext, protected routes, and user operations.

## Use this skill when

- Adding login, signup, or password-change features
- Protecting routes or API endpoints
- Working with user roles/status (admin, pending, suspended)
- Updating user profile data via Supabase Auth
- Debugging auth token issues

## Do not use this skill when

- Building Next.js apps (use `nextjs-supabase-auth` global skill instead)
- Working on non-auth database operations

## Instructions

### Step 1: Client-Side Auth Setup

The Supabase client is in `client/src/lib/supabase.ts`:

```typescript
import { createClient } from "@supabase/supabase-js";

export const supabase = createClient(
  import.meta.env.VITE_SUPABASE_URL,
  import.meta.env.VITE_SUPABASE_ANON_KEY
);
```

### Step 2: Using AuthContext

The `AuthContext` (`client/src/contexts/AuthContext.tsx`) provides:

```typescript
const {
  user,          // Supabase Auth user (email, id, metadata)
  session,       // Current session with access_token
  dbUser,        // App-level user record from /api/user/me
  loading,       // Initial auth check in progress
  authReady,     // Both auth + DB user loaded
  userStatus,    // 'active' | 'pending' | 'suspended'
  isAdmin,       // role === 'admin' || 'super_admin'
  isSuperAdmin,  // role === 'super_admin'
  isPending,     // Non-admin with 'pending' status
  isActive,      // Can access the app
  signUp,        // (email, password) => Promise
  signIn,        // (email, password) => Promise
  signOut,       // () => Promise
} = useAuth();
```

**Key pattern:** Always check `authReady` before rendering protected content:

```tsx
const { user, authReady } = useAuth();
if (!authReady) return <LoadingSpinner />;
if (!user) return <Navigate to="/login" />;
```

### Step 3: Making Authenticated API Calls

Use `apiRequest` from `client/src/lib/queryClient.ts` — it auto-attaches the bearer token:

```typescript
import { apiRequest, queryClient } from "@/lib/queryClient";

// GET (via React Query — token attached automatically)
const { data } = useQuery<MyType>({
  queryKey: ["/api/my-endpoint"],
  enabled: !!user,
});

// POST/PATCH/DELETE
const res = await apiRequest("PATCH", `/api/users/${id}`, { name: "New" });
const data = await res.json();

// Invalidate cache after mutation
queryClient.invalidateQueries({ queryKey: ["/api/my-endpoint"] });
```

### Step 4: Updating User Profile

```typescript
// Change display name (Supabase Auth metadata)
const { error } = await supabase.auth.updateUser({
  data: { full_name: newName },
});

// Change password
const { error } = await supabase.auth.updateUser({
  password: newPassword,
});

// Change email
const { error } = await supabase.auth.updateUser({
  email: newEmail,
});
```

### Step 5: Server-Side Auth Verification

In Express routes (`server/routes.ts`), extract the user from the bearer token:

```typescript
import { createClient } from "@supabase/supabase-js";

const supabaseAdmin = createClient(
  process.env.SUPABASE_URL!,
  process.env.SUPABASE_SERVICE_ROLE_KEY!
);

// Extract user from request
async function getUser(req: Request) {
  const token = req.headers.authorization?.replace("Bearer ", "");
  if (!token) return null;
  const { data: { user } } = await supabaseAdmin.auth.getUser(token);
  return user;
}

// Protect an endpoint
app.get("/api/protected", async (req, res) => {
  const user = await getUser(req);
  if (!user) return res.status(401).json({ message: "Unauthorized" });
  // ... handle request
});
```

### Step 6: Role-Based Access

```typescript
// Client-side
const { isAdmin } = useAuth();
if (!isAdmin) return <Navigate to="/" />;

// Server-side
const dbUser = await db.query.users.findFirst({
  where: eq(users.id, user.id),
});
if (dbUser?.role !== "admin" && dbUser?.role !== "super_admin") {
  return res.status(403).json({ message: "Admin only" });
}
```

## Anti-Patterns

| ❌ Don't | ✅ Do |
|----------|-------|
| Store tokens in localStorage | Let Supabase SDK manage cookies |
| Call `getSession()` in render | Use `useAuth()` hook |
| Skip `authReady` check | Always gate on `authReady` before showing protected UI |
| Use service role key on client | Only use anon key on client, service role on server |

## References

- `client/src/contexts/AuthContext.tsx` — Full AuthContext implementation
- `client/src/lib/queryClient.ts` — apiRequest with auto-auth
- `client/src/lib/supabase.ts` — Supabase client setup
- `server/routes.ts` — Server-side auth patterns
