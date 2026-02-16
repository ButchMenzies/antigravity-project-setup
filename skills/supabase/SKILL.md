---
name: supabase
description: Project-specific Supabase patterns — auth, RLS, table conventions, and client initialisation for IH Coach.
---

# Supabase — IH Coach

Project-specific patterns for working with Supabase in this Next.js application.

## Use this skill when

- Creating or modifying database tables/migrations
- Working with Supabase Auth
- Setting up RLS policies
- Writing queries from client or server code
- Creating Supabase Edge Functions

## Project Context

- **Supabase project ID**: `ovpgtmgoklmjnfnxiejo`
- **Region**: `ap-southeast-1`
- **MCP server**: `supabase-mcp-server` — configured and working
- **Schema file**: `supabase/migrations/001_initial_schema.sql`
- **Env vars** (in `.env.local`):
  - `NEXT_PUBLIC_SUPABASE_URL` — client-safe project URL
  - `NEXT_PUBLIC_SUPABASE_ANON_KEY` — client-safe publishable key

## Table Conventions

### Naming
- Tables: **snake_case plural** (e.g. `teams`, `games`, `training_sessions`)
- Columns: **snake_case** (e.g. `home_score`, `coach_id`, `game_component`)
- IDs: **uuid** via `uuid_generate_v4()`

### Standard columns
Every table should have:
- `id uuid primary key default uuid_generate_v4()`
- `created_at timestamptz default now() not null`
- `updated_at timestamptz default now() not null` (auto-set by trigger)

### Core tables
| Table | PK | Description |
|-------|-----|-------------|
| `profiles` | `id uuid` (FK → `auth.users`) | Coach profile, subscription level |
| `teams` | `id uuid` | Teams with coach owner |
| `team_members` | composite | Team ↔ user membership |
| `venues` | `id uuid` | Shared venues |
| `opposition` | `id uuid` | Shared opposition teams |
| `games` | `id uuid` | Match records with scores |
| `game_ratings` | `id uuid` | Per-component game ratings (1-10) |
| `game_plans` | `id uuid` | Game plans / templates |
| `game_plays` | `id uuid` | Playbook entries |
| `training_sessions` | `id uuid` | Training session records |
| `training_drills` | `id uuid` | Shared drill library |
| `session_drills` | composite | Session ↔ drill join |
| `calendar_events` | `id uuid` | Team calendar events |
| `skill_videos` | `id uuid` | Video library |
| `game_events` | `id uuid` | In-game events (goals, cards) |
| `notifications` | `id uuid` | User notifications |

## RLS Policies

### Ownership model
- **Coach-owned**: teams, games, sessions, game_plans, game_plays, calendar_events → coach_id on teams
- **Shared resources**: venues, opposition, training_drills → any authenticated user
- **Self-owned**: profiles, notifications → auth.uid() = user_id/id
- **Creator-owned**: skill_videos → auth.uid() = creator_id (all authenticated can view)

### Pattern
```sql
-- Coach manages own team resources
CREATE POLICY "Coaches manage games" ON public.games
  FOR ALL USING (
    EXISTS (SELECT 1 FROM public.teams WHERE teams.id = games.team_id AND teams.coach_id = auth.uid())
  );

-- Members can view team resources
CREATE POLICY "Members view games" ON public.games
  FOR SELECT USING (
    EXISTS (SELECT 1 FROM public.team_members WHERE team_members.team_id = games.team_id AND team_members.user_id = auth.uid())
  );
```

## Auth Patterns

### Next.js Browser Client (`lib/supabase/client.ts`)
```typescript
import { createBrowserClient } from '@supabase/ssr';

export function createClient() {
    return createBrowserClient(
        process.env.NEXT_PUBLIC_SUPABASE_URL!,
        process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
    );
}
```

### Next.js Server Client (`lib/supabase/server.ts`)
```typescript
import { createServerClient } from '@supabase/ssr';
import { cookies } from 'next/headers';

export async function createClient() {
    const cookieStore = await cookies();
    return createServerClient(
        process.env.NEXT_PUBLIC_SUPABASE_URL!,
        process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
        {
            cookies: {
                getAll() { return cookieStore.getAll(); },
                setAll(cookiesToSet) {
                    try {
                        cookiesToSet.forEach(({ name, value, options }) =>
                            cookieStore.set(name, value, options)
                        );
                    } catch { /* Server Component — ignore */ }
                },
            },
        }
    );
}
```

### Server Actions (`lib/auth-actions.ts`)
- `login(formData)` — email/password sign in → redirect `/dashboard`
- `signup(formData)` — create account with first/last name metadata → redirect `/dashboard`
- `logout()` — sign out → redirect `/login`
- `resetPassword(formData)` — send reset email

### New User Trigger
```sql
CREATE FUNCTION public.handle_new_user()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO public.profiles (id, email, first_name, last_name)
    VALUES (
        new.id, new.email,
        COALESCE(new.raw_user_meta_data->>'first_name', ''),
        COALESCE(new.raw_user_meta_data->>'last_name', '')
    );
    RETURN new;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;
```

### Middleware Route Protection
- App routes (`/dashboard`, `/teams`, `/games`, etc.) → redirect to `/login` if unauthenticated
- Auth routes (`/login`, `/signup`, `/reset-password`) → redirect to `/dashboard` if authenticated

## Migration Patterns

### Using MCP
```
mcp_supabase-mcp-server_apply_migration(project_id, name, query)  // DDL
mcp_supabase-mcp-server_execute_sql(project_id, query)            // data operations
```

### Column additions
Always use `IF NOT EXISTS` for safety:
```sql
ALTER TABLE games ADD COLUMN IF NOT EXISTS notes text;
```

## Anti-Patterns to Avoid

- **Don't skip RLS** — every new table needs RLS enabled + policies
- **Don't hardcode Supabase URLs** — always use env vars
- **Don't use service key in client code** — only `NEXT_PUBLIC_*` vars
- **Don't mix mock and Supabase data** — DataContext should be the single source during mock phase, Supabase replaces it later
- **Don't store user data in content tables** — keep content and user data separate
