---
name: write-code
description: >-
  Pre-edit verification and implementation discipline for any code change.
  Use when implementing, editing code, modifying, refactoring, or adding features.
---

> **⚠️ First Use — Adapt This Skill**
> This is a generic version with universal principles. Before following the steps below, adapt this skill to your project:
> 1. Read `.agent/AGENT.md` for this project's tech stack and conventions
> 2. Read Step 5 of `.agent/workflows/create-skill.md` for skill structure conventions
> 3. Rewrite the stack-specific sections below (data layer checks, verification commands, build checks) to match your project's actual tools
> 4. Delete this entire notice block when done
> 5. Return to where you were before reading this skill and continue your work
>
> This only happens once. Future reads will use your adapted version.

# Write Code

## When to Use

Every time you are about to write or edit code. This is the foundation skill — other skills (build-component, create-endpoint, fix-bug) build on top of it.

## Before You Touch a File

1. **Read the full file.** Not just the function — the whole module. Understand imports, exports, callers, and data flow.
2. **Check your data layer.** If touching queries or data models, verify the actual schema — don't assume types from variable names. Query your data layer for actual column types, field types, or model definitions.
3. **Grep for callers.** Before changing any function:
   ```bash
   grep -rn "functionName" src/
   ```
   If you change a return type, parameter, or behaviour — update every caller.
4. **Search for existing code.** Before building something new:
   ```bash
   grep -rn "similar keyword" src/
   ```
   If it exists, reuse it. If it almost exists, extend it.

## While Writing

- **Small changes.** Make incremental, testable edits.
- **Match existing patterns.** Read how the codebase does similar things and follow the same pattern.
- **Clean as you go.** If you replace something, delete the old version in the same edit.

## After Writing

- **Run it.** Call the function. Query the data. Verify it works with real data, not assumptions.
- **Check for collateral damage.** Grep for anything that might have broken.
- **Hunt dead code.** Did you leave unused functions, imports, or variables?
- **Run the build.** Does it compile/build cleanly? Fix any warnings while you're here.

## Common Mistakes

1. **Reading only the function, not the file** — you miss how your change affects the overall module
2. **Assuming data types from names** — a field called `goals` could be text, JSON, or an array
3. **Changing a function without checking callers** — you fix one place, break three others
4. **Building new when existing code does the same thing** — duplicate code is future bugs
5. **Marking done without running** — "it should work" has a 50% track record
