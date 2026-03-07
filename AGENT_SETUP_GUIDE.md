# 🚀 Antigravity Agent Setup

> **Paste this into any project chat.** Handles fresh installs and updates automatically.
> Sources everything from GitHub — works on any device.

**Latest version: 9** (2026-03-07)

---

## Step 1: Detect Current State

```bash
cat .agent/version 2>/dev/null || echo "none"
ls .agent/AGENT.md 2>/dev/null
```

### If version = 9 → **ALREADY CURRENT**

Read `.agent/AGENT.md` and `.agent/memory.md`, then proceed with the user's request. You are done with this guide.

### If version = 1, 2, 3, 4, 5, 6, 7, 8, or "none" with `.agent/AGENT.md` → **NEEDS UPDATE**

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
mkdir -p .agent/workflows .agent/skills
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
```

```bash
for skill in /tmp/ag-setup/skills/*/; do
  name=$(basename "$skill")
  case "$name" in bundles) continue;; esac
  mkdir -p ".agent/skills/$name"
  cp -r "$skill"* ".agent/skills/$name/"
done
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

For any skills not already listed in `.agent/skills-catalog.md`:
1. Read its SKILL.md
2. Add to `.agent/skills-catalog.md`

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

echo "9" > .agent/version

Add to `.agent/memory.md` under Session Log:

```markdown
### [TODAY'S DATE] Antigravity updated to v9
**Changes**: Added 14 development skills (create-feature, fix-bug, database-change, build-component, build-page, create-endpoint, performance-audit, auth-flow, payments-stripe, seo-audit, api-design, deploy-vercel, deploy-railway, package-publish). Added roadmap & phase planning system (`conductor/roadmap.md`, `conductor/notes.md`). `/new-track` now has brainstorm gate, roadmap-aware situational awareness, and skills matched by frontmatter during planning. `/end-session` updates roadmap progress and processes notes. See CHANGELOG: https://github.com/ButchMenzies/antigravity-project-setup/blob/main/CHANGELOG.md
```

Tell the user:

```
✅ Antigravity installed/updated to v9!

Installed:
- 15 slash command workflows
- 20+ skills (standard + development skills for features, debugging, database, components, deployment, and more)
- Core rules and Available Commands updated
- NEW: 14 development skills auto-matched during /new-track planning
- NEW: Roadmap & phase planning system (conductor/roadmap.md, conductor/notes.md)
- UPDATED: /new-track — brainstorm gate, roadmap awareness, skill matching
- UPDATED: /end-session — processes notes, updates roadmap progress

**⚠️ Action Required: Close and reopen the project.**
The IDE needs to re-scan to discover the new slash commands.

Next step:
→ Close and reopen the project
→ Then run /setup for interactive onboarding (or /status if already onboarded)
```
