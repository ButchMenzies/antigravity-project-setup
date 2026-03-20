# Project Configuration

## ⚠️ New Chat? Start Here — MANDATORY
1. Read this file completely
2. **Read `.agent/memory.md`** — this contains all decisions, lessons, and preferences from prior sessions. You MUST read it before doing any work.
3. Run `/status` to see where things stand
4. **If slash commands (like /status) don't appear in the autocomplete**, close and reopen the project.

## ⚠️ Core Rules (Always Apply)
1. **Before starting work**: Read the plan if one exists (check `conductor/roadmap.md`, `conductor/tracks/`, or `.agent/current-plan.md`)
2. **Before starting any task**: Scan `.agent/skills/` — read the SKILL.md of any skill relevant to the work
3. **After completing a deliverable**: Update `memory.md` — run `/update-memory`. **Do not skip this.**
4. **Before ending a session**: Run `/end-session` to wrap up. **Do not end a session without updating memory.**
5. **When you notice repeating patterns**: Suggest creating a skill with `/create-skill`
6. **Don't guess about tools, settings, or platform behaviour.** If you're unsure how something works — especially IDE features, APIs, or config options — say so and verify first.
7. **File organization**: Keep deliverables organized in clear folders (e.g. `docs/`, `research/`, `output/`). Use descriptive filenames. Don't create files in the root directory unless they're project-level (README, etc.).
8. **Research discipline**: When researching a topic, cite sources. Distinguish between facts, informed opinions, and speculation. Flag assumptions clearly.
9. **Brainstorm mode**: When the user says "let's brainstorm", "think this through", "don't build yet", or signals they want discussion before action — **stop all implementation and follow the `/brainstorm-lite` workflow.** Do not write code or edit files until the user explicitly says to proceed.
10. **No fake options.** Don't present three options and pick the middle one — that's not analysis, it's theater. If there's a clear best approach, recommend it directly with your reasoning. Only present alternatives when there are genuine trade-offs. Every option you suggest must be something you've verified is actually possible by reading the code, not something a similar app might do.
11. **Do the work yourself.** Don't ask the user to do things you can do. If you can read a file, run a command, check a status, or make an edit — do it. Only involve the user when you genuinely need their input, approval, or access to something you can't reach.
12. **Read the code first.** Before suggesting changes, read the actual code that's relevant. Don't assume how something works based on what a typical app would do — check this specific codebase. If you're fixing a bug, read the module. If you're adding a feature, read the existing patterns. Ground every suggestion in what the code actually says.

## Project Principles

Active design decisions that influence how this project is approached:

<!-- Add principles here as numbered items when a decision should influence future work:
1. **[Title]** — [one-line description of the decision and rationale]
-->

## Development Workflow

1. Plan → `/new-track` (produces detailed implementation plan)
2. Build → `/implement` (execute plan with progress tracking)
3. Review deliverables
4. Wrap up → `/end-session` (update memory, roadmap, product.md)

## Available Commands

### Essential
- `/brainstorm` — brainstorming session (ideas, requirements, conceptual)
- `/brainstorm-lite` — mid-work brainstorm (stop building, think before changing)
- `/new-track` — plan a new piece of work
- `/edit` — revise a plan before implementing
- `/implement` — execute a plan with progress tracking
- `/status` — show project status
- `/update-memory` — log a decision, lesson, or preference
- `/end-session` — wrap up the current session
- `/create-skill` — create a reusable local skill
- `/ux-design` — define your product's design direction (personas, brand, visual identity)
- `/audit` — deep audit of conductor documents against the actual codebase
- `/test` — design and run tests for recent changes or full app QA
- `/security-review` — comprehensive security review (database, API, auth, secrets, infrastructure)

### Additional (install via setup)
- `/capture-target` — capture design data from a live site
- `/recreate-site` — rebuild a site from captured data
- `/compare-site` — fix an existing build against captured target data
- `/offer-strategy` — build a Grand Slam Offer (value stack, bonuses, guarantee, pricing)
- `/lead-strategy` — define lead generation channels, lead magnets, and outreach

---

## Project Overview
*Populate this section during project onboarding.*

## Goals
*What is this project trying to achieve? Populate during onboarding.*

## Audience / Target Market
*Who is this for? Populate during onboarding.*

## Key Resources
*Reference material, links, documents, data sources. Populate during onboarding.*

---

## Resources

- **Local Skills**: `.agent/skills/`
- **Skills Catalog**: `.agent/skills-catalog.md`
- **Memory**: `.agent/memory.md`
- **Global Skills Reference**: https://github.com/sickn33/antigravity-awesome-skills
