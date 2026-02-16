---
name: planning
description: Principles for iterative, grounded planning — plan what you can see, learn first, then re-plan.
---

# Planning

How to plan work well. This skill is about *thinking*, not templates — the workflows handle structure.

## Use this skill when

- Planning any new piece of work (`/new-track`, `/edit`, or ad-hoc)
- Re-evaluating a plan between phases during `/implement`
- A user gives a vague or open-ended request

## Do not use this skill when

- Following an already-approved, detailed plan — just execute it
- The task is trivial and doesn't need a plan (single file fix, config change)

## Planning Principles

### 1. Plan What You Can See (Headlights Planning)

Only plan in detail what you can confidently execute right now. Later phases are intentions, not commitments.

- **Phase 1:** Fully detailed — concrete tasks, clear verification
- **Phase 2:** Outlined with intent, marked as *"to be refined after Phase 1"*
- **Beyond Phase 2:** Directional notes only — "we'll likely need X, Y, Z"

Don't write detailed tasks for phases that depend on things you haven't learned yet. That's fiction, not planning.

### 2. Phase 1 Is Always Discovery

Before building anything, understand what exists. Your first phase should almost always involve reading code, running the app, or auditing the current state.

You can't plan fixes for bugs you haven't found. You can't design an integration without understanding the existing architecture. You can't improve a UI you haven't seen.

### 3. Every Phase Transition Is a Re-Plan

When a phase completes, **stop before starting the next one.** Re-read the plan, consider what you learned during execution, and refine the upcoming phase into concrete tasks. Don't march through a plan that was written before you had the information you have now.

### 4. Improvement Is Not Replacement

When asked to "improve", "fix", or "clean up" something — start by understanding what's specifically wrong. Don't plan a rebuild when a targeted change will do. The user probably wants 3 things fixed, not 30 things redesigned.

### 5. Right-Size the Plan

A plan should be completable in one working session. If it can't — split it into separate tracks. A 6-phase plan spanning multiple sessions is really three plans pretending to be one.

**Be cautious about session sizing.** Agents tend to overestimate how long tasks take. A "phase" with 3-4 concrete tasks often completes faster than expected. Don't split work into too many tracks based on inflated estimates.

### 6. Consider Alternatives

Before committing to your first approach, ask: *Is there a simpler way? Could this use an existing pattern in the codebase? Am I overengineering this?*

## Questions to Ask Yourself

During planning, pause and consider:

- **"What assumptions am I making about the codebase?"** — Have you verified them, or are you guessing?
- **"What could go wrong here?"** — Identify risks before they become surprises
- **"What's the smallest first step that teaches us the most?"** — Discovery phases should maximise learning
- **"Do I actually need to plan this far ahead?"** — If a phase depends on unknown outcomes, sketch it, don't detail it

## Anti-Patterns

- **Waterfall planning** — mapping out 6 detailed phases upfront when you haven't read the code yet
- **Scope explosion** — "integrate Stripe" becoming a 20-task SaaS billing platform plan. Ask what's needed before assuming maximum scope
- **Vague late phases** — phases like "wire everything up" or "final polish" are a sign you're planning past your headlights
- **Planning solutions before finding problems** — creating a fix plan before you've confirmed what's broken

## Project-Specific Planning

This skill covers universal planning principles. For project-specific knowledge (e.g., "database changes always need RLS policies", "UI work requires both iOS and Android testing"), check for skills prefixed with `planning-*` in `.agent/skills/`.

If you notice recurring project-specific patterns during planning, suggest creating a `planning-*` skill via `/create-skill` to capture them for future sessions.

### 7. Design-First for UI Work

Before planning UI features, check if `.agent/ux/` exists. If it does, read `design-direction.md` and `persona.md`. Every screen should contribute to the user's transformation, not just display data. If no design foundation exists, suggest running `/ux-design` before detailed UI planning.

### Do I Need Skills Before Starting?

During planning, ask: *Does this work require domain knowledge I don't have?* Check `.agent/skills/` for relevant skills. If none exist and the work involves a specialised domain (payments, auth, deployment, etc.), consider whether to:

1. Build a project-specific skill first (if the domain will come up again)
2. Research as part of Phase 1 discovery (if it's a one-off)
