---
name: architecture-change
description: >-
  System-level thinking for changes that span multiple modules.
  Use when restructuring, refactoring across boundaries, modifying data flows,
  or changing API contracts.
---

> **⚠️ First Use — Adapt This Skill**
> This is a generic version. Before following the steps below, adapt this skill to your project:
> 1. Read `.agent/AGENT.md` for this project's tech stack and conventions
> 2. Read Step 5 of `.agent/workflows/create-skill.md` for skill structure conventions
> 3. Rewrite the stack-specific sections below (data layer inspection, schema queries, build verification) to match your project's actual tools
> 4. Delete this entire notice block when done
> 5. Return to where you were before reading this skill and continue your work
>
> This only happens once. Future reads will use your adapted version.

# Architecture Change

## When to Use

When a change touches 3+ files, modifies a data flow, changes an API contract, or reorganises module boundaries. NOT for isolated bug fixes or single-component changes.

## Before Proposing Changes

1. **Map the current data flow.** Trace the path from user action → API → data layer → response → UI. Draw it out (in text or mermaid diagram). You cannot change what you don't understand.
2. **Identify all affected modules.** Don't just check direct imports — check transitive dependencies. The function you're changing might be called by a function that's called by three route handlers.
3. **Inspect the data layer.** Query the actual schema — don't rely on type definitions or migration files alone. Verify actual column types, constraints, and relationships.
4. **Read the project principles.** Check `.agent/AGENT.md` → Project Principles. Does your proposed change align with or contradict existing decisions?

## During Design

- **Favour minimal changes.** The simplest change that solves the problem is usually the best.
- **Don't move furniture during a renovation.** If you're fixing a data flow, don't also rename variables and restructure folders.
- **Check for existing abstractions.** Is there already a pattern in the codebase for what you're trying to do?
- **Consider the API contract.** If you change what an endpoint returns, every consumer breaks.

## After Implementation

- **Verify end-to-end.** Architecture changes need full-path testing, not unit checks.
- **Check for orphaned code.** Moving things around often leaves dead imports, unused exports, or abandoned helper functions.
- **Update documentation.** If the change affects how modules interact, update AGENT.md's Architecture section.

## Common Mistakes

1. **Changing the architecture to fix a bug** — most bugs don't need structural changes, just a fix
2. **Not reading all affected files** — changing a shared module without reading all consumers
3. **Designing in isolation** — proposing a new pattern without checking how the codebase does similar things
4. **Over-abstracting** — creating a generic system when a simple solution works for the actual use case
5. **Forgetting to update the Architecture section** — the code changed but AGENT.md still describes the old structure
