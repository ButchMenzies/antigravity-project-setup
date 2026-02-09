# 🚀 Antigravity Agent Setup

> **Paste this into a new project chat to bootstrap Antigravity.**
> Self-contained — works on any device, no local files needed.
> Installs slash command workflows, the skill creator, and a minimal AGENT.md.
> Then run `/setup` for interactive project onboarding.

---

## Step 1: Detect Existing Setup

```bash
ls -la .agent/ 2>/dev/null
ls -la .agent/workflows/ 2>/dev/null
ls -la .agent/skills/ 2>/dev/null
```

### If `.agent/workflows/` exists with 8 workflow files — **FULLY SET UP**

This project already has the slash command system. Skip setup entirely.
Read `.agent/AGENT.md` and `.agent/memory.md`, then proceed with the user's request.

### If `.agent/AGENT.md` exists but `.agent/workflows/` is missing or empty — **NEEDS UPGRADE**

**This project has existing data. Preserve it.**

1. **Run Step 2** to create the directories
2. **Run Step 3** to install workflows
3. **Run Step 4** to install create-skill
4. **Run Step 5** — but **only create files that don't already exist** (see Step 5 rules)
5. **Merge Core Rules into the existing AGENT.md** (see Step 5a)
6. **Scan existing skills** (see Step 5b)
7. Tell the user: *"Your project has been upgraded to use slash commands. Run `/setup` to fill in any gaps, or `/status` to see where things stand."*

### If no `.agent/` directory — **FRESH PROJECT**

Proceed with full bootstrap (Steps 2-6).

---

## Step 2: Create Directory Structure

```bash
mkdir -p .agent/workflows
mkdir -p .agent/skills/create-skill
```

---

## Step 3: Install Workflows

Read each file from the GitHub repo and create it locally. The repo is the source of truth.

**Repo:** `https://github.com/ButchMenzies/antigravity-project-setup`
**Raw base:** `https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main`

Read each of these URLs and write the content to the corresponding local path:

| Remote Path | Local Path |
|------------|------------|
| `.agent/workflows/setup.md` | `.agent/workflows/setup.md` |
| `.agent/workflows/new-track.md` | `.agent/workflows/new-track.md` |
| `.agent/workflows/edit.md` | `.agent/workflows/edit.md` |
| `.agent/workflows/implement.md` | `.agent/workflows/implement.md` |
| `.agent/workflows/status.md` | `.agent/workflows/status.md` |
| `.agent/workflows/update-memory.md` | `.agent/workflows/update-memory.md` |
| `.agent/workflows/end-session.md` | `.agent/workflows/end-session.md` |
| `.agent/workflows/create-skill.md` | `.agent/workflows/create-skill.md` |

For each file: read `{raw base}/{remote path}` → write content to local `{local path}`.

---

## Step 4: Install Create-Skill

Read and create locally:

| Remote Path | Local Path |
|------------|------------|
| `.agent/skills/create-skill/SKILL.md` | `.agent/skills/create-skill/SKILL.md` |

---

## Step 5: Install Templates (CONDITIONAL)

**Rule: Only create files that do NOT already exist.** Existing files contain real project data — never overwrite them.

| Remote Path | Local Path | Install If... |
|------------|------------|---------------|
| `templates/AGENT.md` | `.agent/AGENT.md` | File does NOT exist |
| `templates/memory.md` | `.agent/memory.md` | File does NOT exist |
| `templates/skills-catalog.md` | `.agent/skills-catalog.md` | File does NOT exist |

If `.agent/AGENT.md` already exists → **do NOT overwrite.** Go to Step 5a.
If `.agent/memory.md` already exists → **do NOT overwrite.** Preserve all session history.
If `.agent/skills-catalog.md` already exists → **do NOT overwrite.** Go to Step 5b to reconcile.

### Step 5a: Merge Core Rules (only if AGENT.md already existed)

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
```

Keep all existing content (Project Overview, Tech Stack, etc.) intact below these new sections.

### Step 5b: Scan Existing Skills (only if `.agent/skills/` has entries beyond `create-skill`)

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

## Step 6: Done!

Tell the user:

```
✅ Antigravity bootstrapped!

Installed:
- 8 slash command workflows (/setup, /new-track, /edit, /implement, /status, /update-memory, /end-session, /create-skill)
- Create-skill starter skill
- Minimal AGENT.md with core rules

Next step:
→ Run /setup for interactive project onboarding
```
