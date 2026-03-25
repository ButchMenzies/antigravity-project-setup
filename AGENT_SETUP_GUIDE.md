# 🚀 Antigravity Agent Setup

> **Paste this into any project chat.** Handles fresh installs and updates automatically.
> Sources everything from GitHub — works on any device.

**Latest version: 12** (2026-03-26)

---

## Step 1: Detect Current State

```bash
cat .agent/version 2>/dev/null || echo "none"
ls .agent/AGENT.md 2>/dev/null
```

### If version = 12 → **ALREADY CURRENT**

Read `.agent/AGENT.md` and `.agent/memory.md`, then proceed with the user's request. You are done with this guide.

### If version = 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, or "none" with `.agent/AGENT.md` → **NEEDS UPDATE**

> **⛔ STOP — Do NOT continue with Steps 2-6.**
> Read and follow the `/update` workflow directly from GitHub:
> `https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/.agent/workflows/update.md`
> This handles workflow updates, skill installs, core rules merge, and version tracking. You are done with this guide.

### If no `.agent/` directory → **FRESH PROJECT**

Check if there are source files:

```bash
ls *.* src/ app/ lib/ public/ 2>/dev/null
```

- **Source files exist** → Proceed to Step 2 (full bootstrap)
- **Folder is completely empty:**

  > **⛔ STOP — Do NOT continue with Steps 2-6.**
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

**Step B — Copy essential workflows:**
```bash
cp /tmp/ag-setup/.agent/workflows/new-track.md .agent/workflows/
cp /tmp/ag-setup/.agent/workflows/edit.md .agent/workflows/
cp /tmp/ag-setup/.agent/workflows/implement.md .agent/workflows/
cp /tmp/ag-setup/.agent/workflows/status.md .agent/workflows/
cp /tmp/ag-setup/.agent/workflows/update-memory.md .agent/workflows/
cp /tmp/ag-setup/.agent/workflows/end-session.md .agent/workflows/
cp /tmp/ag-setup/.agent/workflows/brainstorm.md .agent/workflows/
cp /tmp/ag-setup/.agent/workflows/brainstorm-lite.md .agent/workflows/
cp /tmp/ag-setup/.agent/workflows/audit.md .agent/workflows/
cp /tmp/ag-setup/.agent/workflows/create-skill.md .agent/workflows/
cp /tmp/ag-setup/.agent/workflows/ux-design.md .agent/workflows/
cp /tmp/ag-setup/.agent/workflows/test.md .agent/workflows/
cp /tmp/ag-setup/.agent/workflows/security-review.md .agent/workflows/
```

**Step C — Copy skills:**
```bash
for skill in /tmp/ag-setup/skills/*/; do
  name=$(basename "$skill")
  case "$name" in bundles) continue;; esac
  mkdir -p ".agent/skills/$name"
  cp -r "$skill"* ".agent/skills/$name/"
done
```

```bash
if [ -f /tmp/ag-setup/.agent/skills/visual-qa/SKILL.md ]; then
  mkdir -p .agent/skills/visual-qa
  cp /tmp/ag-setup/.agent/skills/visual-qa/SKILL.md .agent/skills/visual-qa/SKILL.md
fi
```

**Step D — Offer additional workflows:**

```
📦 Additional workflows available:

- /capture-target — capture design data from a live site (Visual QA)
- /recreate-site — rebuild a site from captured data (Visual QA)
- /compare-site — fix an existing build against captured target data (Visual QA)
- /offer-strategy — build a Grand Slam Offer (Hormozi framework)
- /lead-strategy — plan lead generation strategy (Hormozi framework)

Install any of these?
1. Install all
2. Let me choose
3. Skip — I don't need any of these
```

**Wait for user response.** Copy selected workflows from `/tmp/ag-setup/.agent/workflows/`.

**Step E — Copy templates (only if files don't exist yet):**
```bash
if [ ! -f .agent/AGENT.md ]; then cp /tmp/ag-setup/templates/AGENT.md .agent/AGENT.md; fi
if [ ! -f .agent/memory.md ]; then cp /tmp/ag-setup/templates/memory.md .agent/memory.md; fi
if [ ! -f .agent/skills-catalog.md ]; then cp /tmp/ag-setup/templates/skills-catalog.md .agent/skills-catalog.md; fi
if [ ! -f .agent/USER_GUIDE.md ]; then cp /tmp/ag-setup/templates/USER_GUIDE.md .agent/USER_GUIDE.md; fi
```

**Step F — Clean up:**
```bash
rm -rf /tmp/ag-setup
```

---

## Step 4: Configure .gitignore

**Do NOT add `.agent/` to `.gitignore`.** If it's already there, remove it and add selective ignores:

```
# Antigravity — keep workflows/skills tracked, ignore personal data
.agent/memory.md
.agent/memory-archive.md
.agent/pending-skill-uploads.md
.agent/current-plan.md
```

---

## Step 5: Write Version + Log + Done

Write the version file:

```bash
echo "12" > .agent/version
```

Add to `.agent/memory.md` under `## Recent Sessions`:

```markdown
### [TODAY'S DATE]
- Antigravity installed v12. Workflows and skills with source tagging, stale file cleanup. See CHANGELOG: https://github.com/ButchMenzies/antigravity-project-setup/blob/main/CHANGELOG.md
```

Tell the user:

```
✅ Antigravity installed v12!

Installed:
- 13 essential workflows + [N] additional workflows
- 20+ skills (standard + development)

Features:
- Two-tier document architecture — active docs + history.md archive
- Condensed memory — Active Lessons + Recent Sessions (5-session rotation)
- Project Principles in AGENT.md — design decisions that influence every feature
- Active Track + Backlog roadmap — replaces numbered phases
- /audit — deep audit of conductor docs against your codebase
- /end-session — updates product.md, roadmap, and memory

⚠️ Action Required: Close and reopen the project.
The IDE needs to re-scan to discover the new slash commands.

Next step:
→ Close and reopen the project
→ Then run /setup for interactive onboarding
```
