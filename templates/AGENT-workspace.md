# Project Configuration

## ⚠️ New Chat? Start Here — MANDATORY
1. Read this file completely
2. **Read `.agent/memory.md`** — this contains all decisions, lessons, and preferences from prior sessions. You MUST read it before doing any work.
3. Run `/status` to see where things stand
4. **If slash commands (like /status) don't appear in the autocomplete**, close and reopen the project.

## ⚠️ Core Rules (Always Apply)
1. **Before starting work**: Read the plan if one exists (check `.agent/current-plan.md` or `conductor/tracks/`)
2. **Before starting any task**: Scan `.agent/skills/` — read the SKILL.md of any skill relevant to the work
3. **After completing a deliverable**: Update `memory.md` — run `/update-memory`. **Do not skip this.**
4. **Before ending a session**: Run `/end-session` to wrap up. **Do not end a session without updating memory.**
5. **When you notice repeating patterns**: Suggest creating a skill with `/create-skill`
6. **Don't guess about tools, settings, or platform behaviour.** If you're unsure how something works — especially IDE features, APIs, or config options — say so and verify first.
7. **File organization**: Keep deliverables organized in clear folders (e.g. `docs/`, `research/`, `output/`). Use descriptive filenames. Don't create files in the root directory unless they're project-level (README, etc.).
8. **Research discipline**: When researching a topic, cite sources. Distinguish between facts, informed opinions, and speculation. Flag assumptions clearly.
9. **Brainstorm mode**: When the user says "let's brainstorm", "think this through", "don't build yet", or signals they want discussion before action — **stop all implementation and follow the `/brainstorm-lite` workflow.** Do not write code or edit files until the user explicitly says to proceed.

## Available Commands
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
- `/offer-strategy` — build a Grand Slam Offer (value stack, bonuses, guarantee, pricing)
- `/lead-strategy` — define lead generation channels, lead magnets, and outreach
- `/capture-target` — capture design data from a live site
- `/recreate-site` — rebuild a site from captured data
- `/compare-site` — fix an existing build against captured target data

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
