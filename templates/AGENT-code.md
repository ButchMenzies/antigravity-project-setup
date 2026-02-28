# Project Configuration

## ⚠️ New Chat? Start Here — MANDATORY
1. Read this file completely
2. **Read `.agent/memory.md`** — this contains all decisions, lessons, and preferences from prior sessions. You MUST read it before doing any work.
3. Run `/status` to see where things stand
4. **If slash commands (like /status) don't appear in the autocomplete**, close and reopen the project.

## ⚠️ Core Rules (Always Apply)
1. **Before implementation**: Read the plan if one exists (check `.agent/current-plan.md` or `conductor/tracks/`)
2. **Before starting any task**: Scan `.agent/skills/` — read the SKILL.md of any skill relevant to the work
3. **After completing a feature/fix**: Update `memory.md` — run `/update-memory`. **Do not skip this.**
4. **Before ending a session**: Run `/end-session` to wrap up. **Do not end a session without updating memory.**
5. **When you notice repeating patterns**: Suggest creating a skill with `/create-skill`
6. **Don't guess about tools, settings, or platform behaviour.** If you're unsure how something works — especially IDE features, APIs, or config options — say so and verify first. Trust your coding knowledge; verify everything else.
7. **Terminal command discipline:**
   - **Before running**: Tell the user what you're about to run and why.
   - **Short commands** (`ls`, `cat`, `mkdir`, file reads): Run synchronously (<2s). If nothing returns, terminate.
   - **Install/build commands** (`npm install`, `git clone`, `npm run build`): 10s initial wait. Poll at most twice (15s each). If still running, tell the user — never silently keep polling.
   - **Dev servers / watchers** (`npm run dev`): 5s initial wait to catch startup errors. Don't poll for completion — these run until stopped.
   - **Never chain commands with `&&`** — if the first command hangs, you lose visibility. Run them separately.
   - **Always use non-interactive mode** (`-y`, `--yes`). If a command produces no output for 10s, assume it's waiting for input — terminate and retry with correct flags.
   - **Maximum 2 status checks on any background command.** After that, terminate and tell the user.
8. **Browser & URLs**: When testing with the browser tool, always share the dev URL with the user afterward so they can check in their own browser. Format: `🔗 Dev server: http://localhost:<port>`
9. **Dev server port**: Always use the port from "Local Development" in this file. Pass it explicitly when starting the dev server (e.g. `--port`, `-p`, or `PORT=` — use the right flag for your framework). Before starting, check if the port is free: `lsof -i :<port> | head -5`. If occupied by a previous dev server (e.g. `node`), ask the user if you should kill it. If occupied by something else, tell the user — don't silently use another port.
10. **Brainstorm mode**: When the user says "let's brainstorm", "think this through", "don't build yet", or signals they want discussion before action — **stop all implementation and follow the `/brainstorm-lite` workflow.** Do not write code or edit files until the user explicitly says to proceed.

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

## Tech Stack
*Populate this section during project onboarding.*

## Architecture
*Populate this section during project onboarding.*

## Conventions
*Populate this section during project onboarding.*

---

## Platform Notes
*If applicable — Replit config, deployment notes, environment variables.*

## Local Development

- **Dev port**: *Populate during /setup*
- **Dev URL**: *Populate during /setup*
- **Start command**: *Populate during /setup*
- **Key scripts**: *Populate during /setup*

---

## Resources

- **Local Skills**: `.agent/skills/`
- **Skills Catalog**: `.agent/skills-catalog.md`
- **Memory**: `.agent/memory.md`
- **Global Skills Reference**: https://github.com/sickn33/antigravity-awesome-skills
