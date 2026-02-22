# 🚀 Antigravity Agent Setup

> **Paste this into any project chat.** Handles fresh installs and updates automatically.
> Sources everything from GitHub — works on any device.

**Latest version: 5** (2026-02-19)

---

## Step 1: Detect Current State

```bash
cat .agent/version 2>/dev/null || echo "none"
ls .agent/AGENT.md 2>/dev/null
```

### If version = 5 → **ALREADY CURRENT**

Read `.agent/AGENT.md` and `.agent/memory.md`, then proceed with the user's request. You are done with this guide.

### If version = 1, 2, 3, 4, or "none" with `.agent/AGENT.md` → **NEEDS UPDATE**

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
  > This handles scaffolding (or workspace setup for non-code projects), bootstrap, and onboarding in one pass. You are done with this guide.

---

## Step 2: Create Directories

```bash
mkdir -p .agent/workflows .agent/skills/planning .agent/skills/ux-design .agent/skills/offer-strategy .agent/skills/lead-strategy .agent/skills/voice-notes-triage .agent/skills/visual-qa
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
rm -f .agent/workflows/setup.md .agent/workflows/new-project.md .agent/workflows/update-guide.md .agent/workflows/critical-analysis.md
cp /tmp/ag-setup/skills/planning/SKILL.md .agent/skills/planning/SKILL.md
cp /tmp/ag-setup/skills/ux-design/SKILL.md .agent/skills/ux-design/SKILL.md
cp -r /tmp/ag-setup/skills/offer-strategy/* .agent/skills/offer-strategy/
cp -r /tmp/ag-setup/skills/lead-strategy/* .agent/skills/lead-strategy/
cp /tmp/ag-setup/skills/voice-notes-triage/SKILL.md .agent/skills/voice-notes-triage/SKILL.md
cp /tmp/ag-setup/.agent/skills/visual-qa/SKILL.md .agent/skills/visual-qa/SKILL.md
```

**Step C — Copy templates (only if files don't exist yet):**
```bash
if [ ! -f .agent/AGENT.md ]; then cp /tmp/ag-setup/templates/AGENT.md .agent/AGENT.md; fi
if [ ! -f .agent/memory.md ]; then cp /tmp/ag-setup/templates/memory.md .agent/memory.md; fi
if [ ! -f .agent/skills-catalog.md ]; then cp /tmp/ag-setup/templates/skills-catalog.md .agent/skills-catalog.md; fi
if [ ! -f .agent/USER_GUIDE.md ]; then cp /tmp/ag-setup/templates/USER_GUIDE.md .agent/USER_GUIDE.md; fi
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

Read the existing `.agent/AGENT.md`. Determine the project type:
- If AGENT.md has **Tech Stack** and **Local Development** sections → code project → use `templates/AGENT-code.md`
- If AGENT.md has **Goals** and **Key Resources** sections → workspace project → use `templates/AGENT-workspace.md`
- If unclear → default to `templates/AGENT-code.md`

Fetch the appropriate template from the Antigravity repo:
`https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/templates/AGENT-code.md`
or
`https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/templates/AGENT-workspace.md`

Search for and **remove** any existing sections whose headers contain "Start Here", "Core Rules", "Rules", or "Available Commands" (case-insensitive). Then copy the "Start Here", "Core Rules", and "Available Commands" sections from the template and add them **at the very top**, before any remaining content.

Keep all existing content (Project Overview, Tech Stack, etc.) intact below these sections.

### Scan existing skills

```bash
ls .agent/skills/
```

For each skill beyond the standard set (planning, ux-design, offer-strategy, lead-strategy, visual-qa):
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
echo "5" > .agent/version
```

Add to `.agent/memory.md` under Session Log:

```markdown
### [TODAY'S DATE] Antigravity updated to v5
**Changes**: Added Visual QA workflow system (/capture-target, /recreate-site, /compare-site) and visual-qa skill. See CHANGELOG: https://github.com/ButchMenzies/antigravity-project-setup/blob/main/CHANGELOG.md
```

Tell the user:

```
✅ Antigravity installed/updated to v5!

Installed:
- 13 slash command workflows
- 6 skills (planning, ux-design, offer-strategy, lead-strategy, voice-notes-triage, visual-qa)
- Core rules and Available Commands updated
- NEW: /capture-target — capture design data from live sites
- NEW: /recreate-site — rebuild a site from captured data in any tech stack
- NEW: /compare-site — fix an existing build against captured target data
- NEW: visual-qa skill — shared engine for applying design data to code

**⚠️ Action Required: Close and reopen the project.**
The IDE needs to re-scan to discover the new slash commands.

Next step:
→ Close and reopen the project
→ Then run /setup for interactive onboarding (or /status if already onboarded)
```
