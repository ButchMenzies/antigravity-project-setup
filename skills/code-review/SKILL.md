---
name: code-review
description: >-
  Review your own work like a senior dev would review a PR.
  Use when verifying changes, auditing code quality, or self-reviewing after implementation.
---

> **⚠️ First Use — Adapt This Skill**
> This is a generic version. Before following the steps below, adapt this skill to your project:
> 1. Read `.agent/AGENT.md` for this project's tech stack and conventions
> 2. Read Step 5 of `.agent/workflows/create-skill.md` for skill structure conventions
> 3. Rewrite the stack-specific sections below (build checks, security checks, type system references) to match your project's actual tools
> 4. Delete this entire notice block when done
> 5. Return to where you were before reading this skill and continue your work
>
> This only happens once. Future reads will use your adapted version.

# Code Review

## When to Use

After completing a change (self-review), during `/audit`, or when asked to review existing code.

## The Review

Read the changed files in full and check each of these:

### Correctness
- [ ] Does the code do what it's supposed to?
- [ ] Are edge cases handled? (null/undefined, empty arrays, missing fields)
- [ ] Do types match reality? (especially data layer types vs code types)
- [ ] Are error paths handled? (try/catch, error responses, fallback UI)

### Side Effects
- [ ] Are callers of changed functions still correct?
- [ ] Could this change break other features?
- [ ] Are there race conditions or timing issues?
- [ ] If this touches state, does the UI react correctly?

### Cleanliness
- [ ] Is there dead code? (unused functions, imports, variables, commented-out blocks)
- [ ] Are there duplicated patterns that should be unified?
- [ ] Do variable/function names clearly describe what they do?
- [ ] Would a new developer understand this code without explanation?

### Security (if applicable)
- [ ] Is user input validated before use?
- [ ] Are auth checks in place on new/modified endpoints?
- [ ] Are secrets kept server-side?
- [ ] Are data queries parameterised? (no string interpolation in queries)

## Common Mistakes

1. **Rubber-stamp self-review** — "I just wrote it so it must be fine" is not a review
2. **Only checking the happy path** — what happens when the data is missing, malformed, or empty?
3. **Ignoring the diff context** — checking only what changed, not how it fits into the surrounding code
4. **Skipping the build check** — compiler/linter errors are free bug detection
