# 🚀 Antigravity Agent Setup

> **Paste this into a new project chat to bootstrap Antigravity.**
> Self-contained — works on any device, no local files needed.
> For existing projects: installs workflows and skills, then run `/setup` for onboarding.
> For blank projects: routes to `/new-project` which handles scaffolding and setup in one pass.

---

## Step 1: Detect Existing Setup

```bash
ls -la .agent/ 2>/dev/null
ls -la .agent/workflows/ 2>/dev/null
ls -la .agent/skills/ 2>/dev/null
```

### If `.agent/workflows/` exists with 9 workflow files — **FULLY SET UP**

This project already has the slash command system. Skip setup entirely.
Read `.agent/AGENT.md` and `.agent/memory.md`, then proceed with the user's request.

### If `.agent/AGENT.md` exists but `.agent/workflows/` is missing or empty — **NEEDS UPGRADE**

**This project has existing data. Preserve it.**

1. **Run Step 2** to create the directories
2. **Run Step 3** to install workflows, skills & templates (existing files are preserved automatically)
3. **Merge Core Rules into the existing AGENT.md** (see Step 4a)
4. **Scan existing skills** (see Step 4b)
5. Tell the user: *"Your project has been upgraded to use slash commands. Run `/setup` to fill in any gaps, or `/status` to see where things stand."*

### If no `.agent/` directory — **FRESH PROJECT**

Check if there are any existing source files:

```bash
ls *.* src/ app/ lib/ public/ 2>/dev/null
```

- **If source files exist** → Proceed with full bootstrap (Steps 2-5), then run `/setup`
- **If folder is completely empty:**

  > **⛔ STOP — Do NOT continue with Steps 2-5.**
  > Read and follow the `/new-project` workflow directly from GitHub:
  > `https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/.agent/workflows/new-project.md`
  > This workflow handles **everything** — scaffolding, Antigravity bootstrap, and project onboarding in one pass. You are done with this guide.

---

## Step 2: Create Directory Structure

```bash
mkdir -p .agent/workflows .agent/skills/planning
```

### Step 2b: Configure .gitignore (IMPORTANT)

**Do NOT add `.agent/` to `.gitignore`.** Blanket-ignoring `.agent/` prevents Antigravity from discovering workflow files for slash command autocomplete.

If `.gitignore` already contains `.agent/` — **remove that line** and replace it with the selective ignores below.

Add these selective ignores to `.gitignore` (if not already present):

```
# Antigravity — keep workflows/skills tracked, ignore personal data
.agent/memory.md
.agent/memory-archive.md
.agent/pending-skill-uploads.md
.agent/current-plan.md
```

---

## Step 3: Install Workflows, Skills & Templates

Clone the repo and copy everything in a few steps — much faster than downloading files individually.

> **Run these as separate commands** — do not chain them with `&&`. Chained commands can hang in agent environments.

**Step A — Clone the repo:**
```bash
git clone --depth 1 https://github.com/ButchMenzies/antigravity-project-setup.git /tmp/ag-setup
```

**Step B — Copy files (workflows, skills, and conditional templates):**
```bash
cp /tmp/ag-setup/.agent/workflows/*.md .agent/workflows/
cp /tmp/ag-setup/skills/planning/SKILL.md .agent/skills/planning/SKILL.md
[ ! -f .agent/AGENT.md ] && cp /tmp/ag-setup/templates/AGENT.md .agent/AGENT.md
[ ! -f .agent/memory.md ] && cp /tmp/ag-setup/templates/memory.md .agent/memory.md
[ ! -f .agent/skills-catalog.md ] && cp /tmp/ag-setup/templates/skills-catalog.md .agent/skills-catalog.md
[ ! -f .agent/USER_GUIDE.md ] && cp /tmp/ag-setup/templates/USER_GUIDE.md .agent/USER_GUIDE.md
```

**Step C — Clean up:**
```bash
rm -rf /tmp/ag-setup
```

---

## Step 4: Post-Install Checks (CONDITIONAL)

If `.agent/AGENT.md` already existed before Step 3 → **Go to Step 4a** to merge core rules.
If `.agent/memory.md` already existed → it was preserved. No action needed.
If `.agent/skills-catalog.md` already existed → **Go to Step 4b** to reconcile.

### Step 4a: Merge Core Rules (only if AGENT.md already existed)

Read the existing `.agent/AGENT.md`. Before adding new sections, search for and **remove** any existing sections whose headers contain the words "Start Here", "Core Rules", "Rules", or "Available Commands" (case-insensitive) — these will be replaced by the new versions. Then add the following sections **at the very top** of the file, before any remaining content:

```markdown
## ⚠️ New Chat? Start Here — MANDATORY
1. Read this file completely
2. **Read `.agent/memory.md`** — this contains all decisions, lessons, and preferences from prior sessions. You MUST read it before doing any work.
3. Run `/status` to see where things stand

## ⚠️ Core Rules (Always Apply)
1. **Before implementation**: Read the plan if one exists (check `.agent/current-plan.md` or `conductor/tracks/`)
2. **Before starting any task**: Scan `.agent/skills/` — read the SKILL.md of any skill relevant to the work
3. **After completing a feature/fix**: Update `memory.md` — run `/update-memory`. **Do not skip this.**
4. **Before ending a session**: Run `/end-session` to wrap up. **Do not end a session without updating memory.**
5. **When you notice repeating patterns**: Suggest creating a skill with `/create-skill`

## Available Commands
- `/setup` — interactive project onboarding (run this first!)
- `/new-track` — plan a new piece of work
- `/edit` — revise a plan before implementing
- `/implement` — execute a plan with progress tracking
- `/status` — show project status
- `/update-memory` — log a decision, lesson, or preference
- `/end-session` — wrap up the current session
- `/create-skill` — create a reusable local skill
- `/new-project` — scaffold a blank project (choose framework, create structure)
```

Keep all existing content (Project Overview, Tech Stack, etc.) intact below these new sections.

### Step 4b: Scan Existing Skills (only if `.agent/skills/` has entries beyond `planning`)

```bash
ls .agent/skills/
```

For each existing skill found:
1. Read its `SKILL.md` to understand what it does
2. Add it to `.agent/skills-catalog.md` if not already listed
3. Check if it's a general-purpose skill (not hyper-specific to this project)
4. If general-purpose, offer to upload it to the Antigravity skills library:

```
I found these existing skills in your project:

| Skill | Description | Upload to library? |
|-------|-------------|-------------------|
| `<skill>` | <from SKILL.md> | Recommended / Skip |

Shall I upload the recommended ones to the Antigravity skills library for use in future projects?

1. Yes, upload all recommended
2. Let me choose
3. Skip — keep them local only
```

---

## Step 5: Done!

Tell the user:

```
✅ Antigravity bootstrapped!

Installed:
- 9 slash command workflows
- Planning skill (iterative planning principles)
- Minimal AGENT.md with core rules

**⚠️ Action Required: Restart this chat session.**
The IDE needs a fresh session to discover the new slash command workflows.

Next step:
→ Start a new chat
→ Run /setup for interactive project onboarding
```
