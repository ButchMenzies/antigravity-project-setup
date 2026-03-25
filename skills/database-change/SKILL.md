---
name: database-change
description: >-
  Handles schema changes with proper migration, RLS policies, type generation,
  and query updates. Use when creating tables, altering columns, adding
  fields, or making any database schema modification.
source: antigravity
---

# Database Change

## When to Use

When the track involves any database schema modification: new tables, new columns, altered constraints, new indexes, or changes to RLS policies. Use alongside **create-feature** if the feature requires schema changes.

## Plan Checklist

Incorporate these items into the implementation plan. The order matters — types must be regenerated before writing code that depends on the new schema.

### Design

- [ ] Document the schema change (what tables/columns, what types, what constraints)
- [ ] Consider the impact on existing data — is a data migration needed?
- [ ] Check for naming consistency with existing tables/columns

### Migration

- [ ] Write the migration SQL
- [ ] If Supabase MCP available: apply via `apply_migration` tool
- [ ] If no MCP: apply via Supabase dashboard or CLI
- [ ] Verify migration applied cleanly (no errors)

### RLS Policies

- [ ] Every new table MUST have at least one RLS policy — no exceptions
- [ ] Review policies on modified tables — do they still make sense with the new columns?
- [ ] Common policies to consider:
  - `SELECT`: who can read these rows?
  - `INSERT`: who can create new rows?
  - `UPDATE`: who can modify rows? Which columns?
  - `DELETE`: who can remove rows?
- [ ] If Supabase MCP available: run `get_advisors` with type `security` to catch gaps

### Type Generation

- [ ] Regenerate TypeScript types from the updated schema
- [ ] If Supabase MCP available: use `generate_typescript_types` tool
- [ ] If no MCP: run `npx supabase gen types typescript` or equivalent
- [ ] Verify new types are imported where needed

### Update Dependent Code

- [ ] Update all queries that touch modified tables/columns
- [ ] Update server actions, API routes, or edge functions
- [ ] Update any type imports or interfaces
- [ ] Check for hardcoded column names that may have changed

### Verification

- [ ] Migration is reversible or has a rollback plan
- [ ] RLS policies work correctly (test as different user roles)
- [ ] TypeScript compiles with no errors against new types
- [ ] Existing features still work after schema change

## Common Mistakes

1. **Forgetting RLS policies on new tables** — the table is publicly readable/writable without them
2. **Not regenerating TypeScript types** — code compiles against stale types, crashes at runtime
3. **Forgetting to update dependent queries** — old code references columns that changed
4. **No data migration plan** — schema changes applied but existing rows have null/invalid values
5. **Testing only as admin** — RLS bugs only appear when testing as a regular user
