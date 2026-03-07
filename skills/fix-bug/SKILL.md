---
name: Fix Bug
description: Structured debugging — reproduce, isolate, fix, verify
triggers: [bug, broken, not working, error, crash, regression, unexpected behavior]
track_types: [bug]
mcps:
  - name: supabase
    usage: check logs, inspect data, verify RLS policies
    required: false
---

# Fix Bug

## When to Use

When the track involves fixing broken functionality, unexpected behavior, errors, or regressions. Always use this for bug tracks — never jump straight to a fix.

## Plan Checklist

Incorporate these items into the implementation plan. Follow the order — skipping steps is how bugs get "fixed" but not actually resolved.

### Reproduce

- [ ] Identify exact steps to trigger the bug
- [ ] Confirm the bug exists in the current codebase (not already fixed)
- [ ] Note the expected behavior vs actual behavior
- [ ] If Supabase MCP available: check relevant service logs (`get_logs`)

### Isolate

- [ ] Identify which layer the bug originates in (database, API, UI, auth, config)
- [ ] Read the relevant code — understand what it's supposed to do before changing it
- [ ] Check recent changes to the affected area (git log)
- [ ] Rule out environment issues (env vars, dependencies, config)

### Root Cause

- [ ] Identify the specific line(s) or logic causing the bug
- [ ] Understand WHY it's wrong, not just WHAT is wrong
- [ ] Check if this same pattern exists elsewhere (the bug might be duplicated)

### Fix

- [ ] Make the minimal change that fixes the root cause
- [ ] Don't refactor or improve unrelated code in the same change
- [ ] If the fix is in shared code, verify it doesn't break other callers

### Verify

- [ ] Bug is fixed — original reproduction steps no longer trigger it
- [ ] No new errors introduced (check console, terminal, build output)
- [ ] Related functionality still works (quick smoke test of adjacent features)
- [ ] TypeScript compiles cleanly

## Common Mistakes

1. **Jumping straight to a fix** without reproducing first — you might fix the wrong thing
2. **Fixing symptoms instead of root cause** — the bug comes back in a different form
3. **Over-fixing** — refactoring the whole module when one line was wrong
4. **Not checking for duplicated patterns** — if the bug was in copied code, the copy still has it
5. **Assuming the error message is accurate** — the real cause is often one layer deeper
