---
version: 2
description: Update an existing Antigravity project to the latest version — workflows, skills, core rules
---

# Update Project

Update an existing Antigravity-powered project to the latest version. Handles workflow installation (essential + additional), skill updates, core rules merge, document structure migration, and version tracking.

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

- end-session.md (v1 → v2) — now uses Recent Sessions, Active Track, removed tech-stack step
- implement.md (v1 → v2) — Active Track, Active Lessons, Project Principles
- new-track.md (v1 → v2) — rewritten for Active Track + Backlog roadmap
- status.md (v1 → v2) — removed tech-stack step, Active Track format
- update-memory.md (v1 → v2) — routes decisions to Project Principles
- audit.md (v1 → v2) — removed tech-stack references
- security-review.md (v1 → v2) — updated references
- [etc.]

⚠️ If you've customized any of these locally, updating will replace your changes.
The new workflows reference a different document structure (Active Lessons, Recent Sessions,
Active Track). Keeping old versions may cause workflows to reference sections that no longer exist.

1. Update all (recommended)
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
- /offer-strategy — build a Grand Slam Offer (value stack, bonuses, guarantee, pricing)
- /lead-strategy — define lead generation channels, lead magnets, and outreach

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

### If AGENT.md was just created → Skip to Step 6b

The template already has the latest core rules.

### If AGENT.md already existed → Merge

Read the existing `.agent/AGENT.md`. Determine project type:
- Has **Tech Stack** and **Local Development** sections → code project → use `templates/AGENT-code.md`
- Has **Goals** and **Key Resources** sections → workspace project → use `templates/AGENT-workspace.md`
- If unclear → default to `templates/AGENT-code.md`

Read the appropriate template from `/tmp/ag-setup/templates/`.

Search for and **remove** any existing sections whose headers contain "Start Here", "Core Rules", "Rules", or "Available Commands" (case-insensitive). Copy the "Start Here", "Core Rules", "Project Principles", "Development Workflow", and "Available Commands" sections from the template and add them **at the very top**, before any remaining content.

**Important for Project Principles:** If the existing AGENT.md already has a `## Project Principles` section with content, **preserve its content** — don't replace it with the empty template stub. Merge by keeping the template's header and the user's existing principles.

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

## Step 6b: Document Structure Migration (v10 → v11)

**Only run this step when upgrading from version 10 or earlier.** Check `.agent/version` to determine the current version.

This step migrates the document structure from the v10 format to the v11 two-tier architecture (active + archive). All changes are interactive — the user approves each migration.

### 6b.1: Memory Structure Migration

Read `.agent/memory.md`. Check if it uses the v10 structure (has `## Key Decisions`, `## Lessons Learned`, and/or `## Session Log` with a table format).

If v10 structure detected, present:

```
📋 Memory Structure Migration (v10 → v11)

Your memory.md currently uses the v10 structure:
- Key Decisions: [N] entries
- Lessons Learned: [N] entries
- Session Log: [N] rows

The v11 structure separates active context from archives:

• Key Decisions → design-influencing decisions go to AGENT.md Project Principles
                → historical decisions go to conductor/history.md
• Lessons Learned → renamed to Active Lessons (only keep genuine traps)
• Session Log → condensed Recent Sessions (last 5 sessions, rest archived)
• User Preferences → unchanged

How would you like to proceed?
1. Auto-migrate — move everything, I'll curate later
2. Guided migration — walk me through each section
3. Skip for now (⚠️ workflows will reference new section names)
```

**Wait for user response.**

**If auto-migrate:**
1. Rename `## Lessons Learned` → `## Active Lessons` (keep all entries as-is)
2. Convert `## Session Log` table → `## Recent Sessions`:
   - Take the last 5 table rows and convert each to:
     ```markdown
     ### [Date]
     - [Summary from table row]
     ```
   - Move all earlier rows to `conductor/history.md` → Archived Session Log table
3. Move all `## Key Decisions` entries to `conductor/history.md` → Archived Decisions
4. Remove the `## Key Decisions` section from memory.md
5. Ensure `## User Preferences` is preserved as-is

**If guided:**
1. Walk through Key Decisions one at a time:
   - "Does this decision influence how future features should be built?"
   - Yes → Add to `AGENT.md` → `## Project Principles`
   - No → Archive to `conductor/history.md`
2. Walk through Lessons Learned one at a time:
   - "Is this still a genuine trap — something that could bite you again?"
   - Yes → Keep in `## Active Lessons`
   - No → Archive to `conductor/history.md`
3. Auto-convert Session Log (mechanical — no judgment needed):
   - Last 5 → Recent Sessions
   - Rest → history.md Archived Session Log

### 6b.2: Conductor Structure Migration

If `conductor/` directory exists:

**history.md:**
- If `conductor/history.md` doesn't exist, create it with the empty scaffold:
  ```markdown
  # [Project Name] — Build History

  > Archive of completed work, past decisions, and session logs.
  > Active reference lives in product.md, roadmap.md, .agent/memory.md.

  ---

  ## Build Timeline

  | Phase | Period | What Was Built |
  |-------|--------|---------------|

  ## Archived Decisions

  ## Archived Lessons Learned

  ## Archived Session Log

  | Date | Summary |
  |------|---------|

  ## Archived Design Documents
  ```

**tech-stack.md:**
- If `conductor/tech-stack.md` exists:
  ```
  📦 Tech Stack Migration

  Your project has a standalone tech-stack.md. In v11, this info lives in
  product.md under "## Infrastructure Services".

  1. Merge into product.md and delete tech-stack.md
  2. Keep tech-stack.md as-is (⚠️ /end-session and /audit won't update it)
  3. Skip — I'll handle this manually
  ```
  **Wait for user response.** If merge: read tech-stack.md content, add an `## Infrastructure Services` section to product.md with the content, then delete tech-stack.md.

**strategy.md:**
- If `conductor/strategy.md` exists (non-code projects):
  ```
  📦 Strategy Migration

  Your project has a standalone strategy.md. In v11, strategy info lives in
  product.md under appropriate sections.

  1. Merge into product.md and delete strategy.md
  2. Keep strategy.md as-is
  3. Skip — I'll handle this manually
  ```
  **Wait for user response.**

**roadmap.md:**
- If `conductor/roadmap.md` exists and uses emoji phase format (contains 🔄 or ✅ or `## Phase`):
  ```
  📦 Roadmap Migration

  Your roadmap uses the phase/emoji format. The v11 format uses Active Track + themed Backlog.

  1. Convert automatically:
     - Completed phases (✅) → history.md Build Timeline
     - Active phase (🔄) → Active Track section
     - Future phases → Backlog themes
  2. Skip — I'll restructure it manually
  ```
  **Wait for user response.** If convert: restructure the roadmap as described.

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
echo "11" > .agent/version
```

Add to `.agent/memory.md` under `## Recent Sessions`:

```markdown
### [TODAY'S DATE]
- Antigravity updated to v11. Document architecture redesign: two-tier active/archive system, condensed memory (Active Lessons + Recent Sessions), Project Principles in AGENT.md, history.md archive
```

Tell the user:

```
✅ Antigravity updated to v11!

Updated:
- [N] essential workflows (version 2 — Active Track, Active Lessons, Recent Sessions)
- [N] additional workflows (your selection)
- 20+ skills (standard + development)
- Core rules, Project Principles, and Available Commands updated

What's new in v11:
- Two-tier document architecture — active docs (memory, roadmap, product) + archive (history.md)
- Condensed memory — Active Lessons + Recent Sessions (5-session rotation) replace old append-only lists
- Project Principles in AGENT.md — design decisions that influence every feature
- Active Track + Backlog roadmap — replaces numbered phases with themed backlog
- tech-stack.md merged into product.md — one less file to maintain

⚠️ Action Required: Close and reopen the project.
The IDE needs to re-scan to discover the new slash commands.

Next step:
→ Close and reopen the project
→ Then run /status to see where things stand
```
