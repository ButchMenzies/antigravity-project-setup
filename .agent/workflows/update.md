---
version: 1
description: Update an existing Antigravity project to the latest version — workflows, skills, core rules
---

# Update Project

Update an existing Antigravity-powered project to the latest version. Handles workflow installation (essential + additional), skill updates, core rules merge, and version tracking.

> This workflow is fetched from GitHub by the setup guide when it detects an outdated version. It is NOT installed into user projects.

---

## Step 1: Clone Repository

```bash
git clone --depth 1 https://github.com/ButchMenzies/antigravity-project-setup.git /tmp/ag-setup
```

---

## Step 2: Install Essential Workflows

Essential workflows: `new-track`, `edit`, `implement`, `status`, `update-memory`, `end-session`, `brainstorm`, `brainstorm-lite`, `audit`, `create-skill`, `ux-design`, `test`, `security-review`.

For each essential workflow, compare versions before installing:

1. Read the `version:` field from the YAML frontmatter of the **repo** copy (`/tmp/ag-setup/.agent/workflows/[name].md`)
2. Read the `version:` field from the **local** copy (`.agent/workflows/[name].md`) — if it exists
3. Apply this logic:
   - **Local doesn't exist** → install (new workflow)
   - **Local has no version field** → overwrite (pre-versioning install)
   - **Local version < repo version** → update available — add to update list
   - **Local version = repo version** → skip (already current)

After checking all essential workflows, if any have updates available, present them:

```
📋 Workflow Updates Available

These essential workflows have newer versions:

- end-session.md (v1 → v2) — now updates product.md, tech-stack.md, tiered roadmap
- status.md (v1 → v2) — now reads all conductor documents
- [etc.]

⚠️ If you've customized any of these locally, updating will replace your changes.

1. Update all
2. Let me choose which to update
3. Skip workflow updates (keep current versions)
```

**Wait for user response.** Only overwrite the workflows the user approves.

---

## Step 3: Install Skills

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

---

## Step 4: Manage Additional Workflows

Additional workflows: `capture-target`, `recreate-site`, `compare-site`, `offer-strategy`, `lead-strategy`.

### 4a. Check existing additional workflows

```bash
ls .agent/workflows/capture-target.md .agent/workflows/recreate-site.md .agent/workflows/compare-site.md .agent/workflows/offer-strategy.md .agent/workflows/lead-strategy.md 2>/dev/null
```

For each **installed** additional workflow, compare the local version against the repo version (same logic as Step 2). Categorise each:
- **Has update** — local version < repo version
- **Current** — local version = repo version
- **No version** — pre-versioning install, treat as needing update

Present findings:

```
📦 Additional Workflows Status

Installed (current):
- /capture-target (v1)
- /compare-site (v1)

Update available:
- /recreate-site (v1 → v2) — ⚠️ updating will replace any local changes

Not installed:
- /offer-strategy — build a Grand Slam Offer (Hormozi framework)
- /lead-strategy — plan lead generation strategy (Hormozi framework)

What would you like to do?
1. Update outdated + install missing
2. Update outdated only
3. Let me choose
4. Skip — keep everything as-is
```

**Wait for user response.** Apply changes based on selection.

If no additional workflows are installed and none have updates, just offer them:

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

---

## Step 5: Copy Templates (only if missing)

```bash
if [ ! -f .agent/AGENT.md ]; then cp /tmp/ag-setup/templates/AGENT.md .agent/AGENT.md; fi
if [ ! -f .agent/memory.md ]; then cp /tmp/ag-setup/templates/memory.md .agent/memory.md; fi
if [ ! -f .agent/skills-catalog.md ]; then cp /tmp/ag-setup/templates/skills-catalog.md .agent/skills-catalog.md; fi
if [ ! -f .agent/USER_GUIDE.md ]; then cp /tmp/ag-setup/templates/USER_GUIDE.md .agent/USER_GUIDE.md; fi
```

---

## Step 6: Merge Core Rules into AGENT.md

### If AGENT.md was just created → Skip to Step 7

The template already has the latest core rules.

### If AGENT.md already existed → Merge

Read the existing `.agent/AGENT.md`. Determine project type:
- Has **Tech Stack** and **Local Development** sections → code project → use `templates/AGENT-code.md`
- Has **Goals** and **Key Resources** sections → workspace project → use `templates/AGENT-workspace.md`
- If unclear → default to `templates/AGENT-code.md`

Read the appropriate template from `/tmp/ag-setup/templates/`.

Search for and **remove** any existing sections whose headers contain "Start Here", "Core Rules", "Rules", or "Available Commands" (case-insensitive). Copy the "Start Here", "Core Rules", and "Available Commands" sections from the template and add them **at the very top**, before any remaining content.

Keep all existing content (Project Overview, Tech Stack, etc.) intact below.

### Update Available Commands for installed workflows

After merging, check which additional workflows are installed. If any additional workflow is installed, ensure its command appears in the Available Commands section. If any additional workflow was removed, ensure its command is removed.

### Scan skills

```bash
ls .agent/skills/
```

For any skills not already listed in `.agent/skills-catalog.md`:
1. Read its SKILL.md
2. Add to `.agent/skills-catalog.md`

---

## Step 7: Clean Up

```bash
rm -rf /tmp/ag-setup
```

---

## Step 8: Configure .gitignore

**Do NOT add `.agent/` to `.gitignore`.** If it's already there, remove it and add selective ignores:

```
# Antigravity — keep workflows/skills tracked, ignore personal data
.agent/memory.md
.agent/memory-archive.md
.agent/pending-skill-uploads.md
.agent/current-plan.md
```

---

## Step 9: Write Version + Log + Done

Write the version file:

```bash
echo "10" > .agent/version
```

Add to `.agent/memory.md` under Session Log:

```markdown
| [TODAY'S DATE] | Antigravity updated to v10. Essential workflows (13) + selected additional workflows installed. New: `/audit` workflow for conductor document reconciliation. Enhanced `/end-session` with product.md, tech-stack.md, and tiered roadmap updates. Enhanced `/status` reads all conductor docs. `product.md` now uses Vision/Current State model. See CHANGELOG: https://github.com/ButchMenzies/antigravity-project-setup/blob/main/CHANGELOG.md |
```

Tell the user:

```
✅ Antigravity updated to v10!

Updated:
- 13 essential workflows (always installed)
- [N] additional workflows (your selection)
- 20+ skills (standard + development)
- Core rules and Available Commands updated

What's new in v10:
- /audit — deep audit of conductor docs against your codebase
- Enhanced /end-session — now updates product.md, tech-stack.md, and tiered roadmap changes
- Enhanced /status — reads all conductor documents
- product.md Vision/Current State model — separates aspirations from reality
- Workflow categories — essentials always installed, additionals offered as options

⚠️ Action Required: Close and reopen the project.
The IDE needs to re-scan to discover the new slash commands.

Next step:
→ Close and reopen the project
→ Then run /status to see where things stand
```
