# 🚀 Antigravity Agent Setup

> **Paste this into any project chat.** Handles fresh installs and updates automatically.
> Sources everything from GitHub — works on any device.

**Latest version: 3** (2026-02-17)

---

## Step 1: Detect Current State

```bash
cat .agent/version 2>/dev/null || echo "none"
ls .agent/AGENT.md 2>/dev/null
```

### If version = 3 → **ALREADY CURRENT**

Read `.agent/AGENT.md` and `.agent/memory.md`, then proceed with the user's request. You are done with this guide.

### If version = 1, 2, or "none" with `.agent/AGENT.md` → **NEEDS UPDATE**

Proceed to Step 2. Existing data will be preserved.

### If no `.agent/` directory → **FRESH PROJECT**

Check if there are source files:

```bash
ls *.* src/ app/ lib/ public/ 2>/dev/null
```

- **Source files exist** → Proceed to Step 2 (full bootstrap)
- **Folder is completely empty:**

  > **⛔ STOP — Do NOT continue with Steps 2-5.**
  > Read and follow the `/new-project` workflow directly from GitHub:
  > `https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/.agent/workflows/new-project.md`
  > This handles scaffolding, bootstrap, and onboarding in one pass. You are done with this guide.

---

## Step 2: Create Directories

```bash
mkdir -p .agent/workflows .agent/skills/planning .agent/skills/ux-design .agent/skills/offer-strategy .agent/skills/lead-strategy .agent/skills/voice-notes-triage
```

---

## Step 3: Install from GitHub

> **Run these as separate commands** — do not chain with `&&`.

**Step A — Clone:**
```bash
git clone --depth 1 https://github.com/ButchMenzies/antigravity-project-setup.git /tmp/ag-setup
```

**Step B — Copy workflows and skills:**
```bash
cp /tmp/ag-setup/.agent/workflows/*.md .agent/workflows/
rm -f .agent/workflows/setup.md
cp /tmp/ag-setup/skills/planning/SKILL.md .agent/skills/planning/SKILL.md
cp /tmp/ag-setup/skills/ux-design/SKILL.md .agent/skills/ux-design/SKILL.md
cp -r /tmp/ag-setup/skills/offer-strategy/* .agent/skills/offer-strategy/
cp -r /tmp/ag-setup/skills/lead-strategy/* .agent/skills/lead-strategy/
cp /tmp/ag-setup/skills/voice-notes-triage/SKILL.md .agent/skills/voice-notes-triage/SKILL.md
```

**Step C — Copy templates (only if files don't exist yet):**
```bash
[ ! -f .agent/AGENT.md ] && cp /tmp/ag-setup/templates/AGENT.md .agent/AGENT.md
[ ! -f .agent/memory.md ] && cp /tmp/ag-setup/templates/memory.md .agent/memory.md
[ ! -f .agent/skills-catalog.md ] && cp /tmp/ag-setup/templates/skills-catalog.md .agent/skills-catalog.md
[ ! -f .agent/USER_GUIDE.md ] && cp /tmp/ag-setup/templates/USER_GUIDE.md .agent/USER_GUIDE.md
```

**Step D — Clean up:**
```bash
rm -rf /tmp/ag-setup
```

---

## Step 4: Update AGENT.md (CONDITIONAL)

### If AGENT.md was just created (fresh install) → Skip to Step 5

The template already has the latest core rules and commands.

### If AGENT.md already existed → Merge core rules

Read the existing `.agent/AGENT.md`. Search for and **remove** any existing sections whose headers contain "Start Here", "Core Rules", "Rules", or "Available Commands" (case-insensitive) — these will be replaced. Then add these sections **at the very top**, before any remaining content:

```markdown
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

## Available Commands
- `/new-track` — plan a new piece of work
- `/edit` — revise a plan before implementing
- `/implement` — execute a plan with progress tracking
- `/status` — show project status
- `/update-memory` — log a decision, lesson, or preference
- `/end-session` — wrap up the current session
- `/create-skill` — create a reusable local skill
- `/new-project` — scaffold a blank project (choose framework, create structure)
- `/ux-design` — define your product's design direction (personas, brand, visual identity)
- `/offer-strategy` — build a Grand Slam Offer (value stack, bonuses, guarantee, pricing)
- `/lead-strategy` — define lead generation channels, lead magnets, and outreach
```

Keep all existing content (Project Overview, Tech Stack, etc.) intact below these sections.

### Scan existing skills

```bash
ls .agent/skills/
```

For each skill beyond the standard set (planning, ux-design, offer-strategy, lead-strategy):
1. Read its SKILL.md
2. Add to `.agent/skills-catalog.md` if not listed

---

## Step 5: Configure .gitignore

**Do NOT add `.agent/` to `.gitignore`.** If it's already there, remove it and add selective ignores:

```
# Antigravity — keep workflows/skills tracked, ignore personal data
.agent/memory.md
.agent/memory-archive.md
.agent/pending-skill-uploads.md
.agent/current-plan.md
```

---

## Step 6: Write Version + Log + Done

Write the version file:

```bash
echo "3" > .agent/version
```

Add to `.agent/memory.md` under Session Log:

```markdown
### [TODAY'S DATE] Antigravity updated to v3
**Changes**: All workflows and skills updated to latest. See CHANGELOG: https://github.com/ButchMenzies/antigravity-project-setup/blob/main/CHANGELOG.md
```

Tell the user:

```
✅ Antigravity installed/updated to v3!

Installed:
- 12 slash command workflows (including /ux-design, /offer-strategy, /lead-strategy)
- 4 skills (planning, ux-design, offer-strategy, lead-strategy)
- Core rules and Available Commands updated

**⚠️ Action Required: Close and reopen the project.**
The IDE needs to re-scan to discover the new slash commands.

Next step:
→ Close and reopen the project
→ Then run /setup for interactive onboarding (or /status if already onboarded)
```
