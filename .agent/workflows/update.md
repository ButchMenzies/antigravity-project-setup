---
version: 3
description: Update an existing Antigravity project to the latest version — workflows, skills, core rules
---

# Update Project

Update an existing Antigravity-powered project to the latest version. Handles workflow installation (essential + additional), skill updates, core rules merge, stale file cleanup, document structure migration, and version tracking.

> This workflow is fetched from GitHub by the setup guide when it detects an outdated version. It is NOT installed into user projects.

---

## Step 1: Clone Repository

```bash
git clone --depth 1 https://github.com/ButchMenzies/antigravity-project-setup.git /tmp/ag-setup
```

---

## Step 2: Detect Project Type

Read `.agent/AGENT.md` and determine the project type:
- Has **Tech Stack** and **Local Development** sections → **code project**
- Has **Goals** and **Key Resources** sections (without Tech Stack) → **workspace project**
- If unclear → default to **code project**

This determines which workflows and skills are installed in Steps 3-5.

---

## Step 3: Install Essential Workflows

Essential workflows depend on project type:

| Workflow | Code | Workspace |
|----------|:---:|:---:|
| `new-track` | ✅ | ✅ |
| `implement` | ✅ | ✅ |
| `edit` | ✅ | ✅ |
| `brainstorm` | ✅ | ✅ |
| `brainstorm-lite` | ✅ | ✅ |
| `status` | ✅ | ✅ |
| `update-memory` | ✅ | ✅ |
| `end-session` | ✅ | ✅ |
| `create-skill` | ✅ | ✅ |
| `audit` | ✅ | — |
| `test` | ✅ | — |
| `security-review` | ✅ | — |
| `ux-design` | ✅ | — |
| `review` | — | ✅ |
| `workspace-audit` | — | ✅ |
| `brand-design` | — | ✅ |

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

## Step 4: Install Skills

For **code projects**: copy all skills from the repo (current behaviour).

```bash
for skill in /tmp/ag-setup/skills/*/; do
  name=$(basename "$skill")
  case "$name" in bundles) continue;; esac
  mkdir -p ".agent/skills/$name"
  cp -r "$skill"* ".agent/skills/$name/"
done
```

For **workspace projects**: copy only workspace-relevant skills.

```bash
for skill in planning copywriting ux-design voice-notes-triage; do
  if [ -d "/tmp/ag-setup/skills/$skill" ]; then
    mkdir -p ".agent/skills/$skill"
    cp -r "/tmp/ag-setup/skills/$skill/"* ".agent/skills/$skill/"
  fi
done
```

Also remove any dev skills that were previously installed in workspace projects (from earlier updates that didn't filter):

```
If workspace project has dev skills installed (api-design, auth-flow, browser-testing, etc.):

⚠️ Your workspace has [N] development skills installed that aren't relevant:
[list skills]

1. Remove them — clean up the workspace
2. Keep them — no harm, just extra files
```

**Wait for user response.**

```bash
if [ -f /tmp/ag-setup/.agent/skills/visual-qa/SKILL.md ]; then
  mkdir -p .agent/skills/visual-qa
  cp /tmp/ag-setup/.agent/skills/visual-qa/SKILL.md .agent/skills/visual-qa/SKILL.md
fi
```

---

## Step 5: Manage Additional Workflows

Additional workflows depend on project type:

**Code projects:** `capture-target`, `recreate-site`, `compare-site`, `offer-strategy`, `lead-strategy`.

**Workspace projects:** `offer-strategy`, `lead-strategy`.

### 5a. Check existing additional workflows

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

For **workspace projects**, use a simplified offer (no Visual QA workflows):

```
📦 Additional workflows available:

- /offer-strategy — build a Grand Slam Offer (Hormozi framework)
- /lead-strategy — plan lead generation strategy (Hormozi framework)

Install any of these?
1. Install all
2. Skip — I don't need any of these
```

**Wait for user response.**

Also check if workspace projects have code-only workflows installed from previous updates. If `test.md`, `security-review.md`, or `audit.md` exist in a workspace project:

```
⚠️ Your workspace has code-specific workflows installed:
[list found: test.md, security-review.md, audit.md]

These have workspace equivalents (review.md, workspace-audit.md, brand-design.md)
that were installed in Step 3.

1. Remove the code workflows — they aren't useful here
2. Keep them — no harm done
```

**Wait for user response.**

---

## Step 5b: Cleanup Stale Files

After installing and updating workflows and skills, clean up files that shouldn't be in this project. This handles files leaked by older Antigravity versions and identifies user-created files.

### Workflow Cleanup

Scan all `.md` files in `.agent/workflows/`. For each file, read its YAML frontmatter and categorise it:

**Category 1 — Tagged Antigravity, wrong project type:**
If the file has `source: antigravity` in its frontmatter but is NOT in this project type's install list (from the Step 3 table) → **remove silently**. These were installed by Antigravity but shouldn't be here for this project type.

**Category 2 — Known Antigravity name, no tag (legacy):**
If the file has NO `source: antigravity` tag but its filename matches any known Antigravity workflow name → **remove silently**. These leaked from pre-v12 installs.

Known Antigravity workflow names (all 26):
`new-track`, `implement`, `edit`, `brainstorm`, `brainstorm-lite`, `status`, `update-memory`, `end-session`, `create-skill`, `audit`, `test`, `security-review`, `ux-design`, `review`, `workspace-audit`, `brand-design`, `capture-target`, `recreate-site`, `compare-site`, `offer-strategy`, `lead-strategy`, `setup`, `new-project`, `update`, `update-guide`, `critical-analysis`

**Category 3 — Unknown (not Antigravity):**
If the file has NO `source: antigravity` tag AND its filename does NOT match any known Antigravity name → **flag to user**.

If any Category 1 or 2 files were removed, or Category 3 files exist, present results:

```
🧹 Workflow Cleanup

Removed (Antigravity files not needed for your project type):
- [list removed files, if any]

Unknown workflows (not maintained by Antigravity):
- [list unknown files, if any]

These aren't part of the Antigravity system. If you didn't create
them yourself, consider removing them.

1. Keep them all
2. Let me choose which to remove
3. Remove all unknown files
```

**Wait for user response.** Apply their choice.

If no files to remove and no unknown files → skip silently.

### Skill Cleanup

Apply the same three-category logic to `.agent/skills/`. For each subdirectory, read `SKILL.md` frontmatter:

- **Category 1** — has `source: antigravity` but not in this project type's skill install list → remove the entire skill directory
- **Category 2** — no tag, matches a known Antigravity skill name → remove (legacy)
- **Category 3** — no tag, unknown name → flag to user

Known Antigravity skill names:
`api-design`, `auth-flow`, `browser-testing`, `build-component`, `build-page`, `copywriting`, `create-endpoint`, `create-feature`, `database-change`, `deploy-railway`, `deploy-vercel`, `fix-bug`, `lead-strategy`, `offer-strategy`, `package-publish`, `payments-stripe`, `performance-audit`, `planning`, `seo-audit`, `ux-design`, `voice-notes-triage`, `visual-qa`

Present skill cleanup results the same way as workflow cleanup (only if there are items to report).

---

## Step 6: Copy Templates (only if missing)

```bash
if [ ! -f .agent/AGENT.md ]; then cp /tmp/ag-setup/templates/AGENT.md .agent/AGENT.md; fi
if [ ! -f .agent/memory.md ]; then cp /tmp/ag-setup/templates/memory.md .agent/memory.md; fi
if [ ! -f .agent/skills-catalog.md ]; then cp /tmp/ag-setup/templates/skills-catalog.md .agent/skills-catalog.md; fi
if [ ! -f .agent/USER_GUIDE.md ]; then cp /tmp/ag-setup/templates/USER_GUIDE.md .agent/USER_GUIDE.md; fi
```

---

## Step 7: Merge Core Rules into AGENT.md

### If AGENT.md was just created → Skip to Step 7b

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

## Step 7b: Document Structure Migration (v10 → v11)

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

## Step 8: Clean Up

```bash
rm -rf /tmp/ag-setup
```

---

## Step 9: Configure .gitignore

**Do NOT add `.agent/` to `.gitignore`.** If it's already there, remove it and add selective ignores:

```
# Antigravity — keep workflows/skills tracked, ignore personal data
.agent/memory.md
.agent/memory-archive.md
.agent/pending-skill-uploads.md
.agent/current-plan.md
```

---

## Step 10: Write Version + Log + Done

Write the version file:

```bash
echo "12" > .agent/version
```

Add to `.agent/memory.md` under `## Recent Sessions`:

```markdown
### [TODAY'S DATE]
- Antigravity updated to v12. Workflow/skill sync: source tagging, stale file cleanup, project-type-aware distribution
```

Tell the user:

```
✅ Antigravity updated to v12!

Updated:
- [N] essential workflows updated
- [N] additional workflows (your selection)
- Skills synced for your project type
- Stale/leaked files cleaned up
- Core rules, Project Principles, and Available Commands updated

What's new in v12:
- Source tagging — all Antigravity-managed files now have `source: antigravity` in frontmatter
- Stale file cleanup — setup-only workflows leaked by older versions are removed
- Project-type-aware sync — only workflows/skills relevant to your project type are kept
- Anthropic format alignment — all skills and workflows now follow the Agent Skills spec (lowercase names, third-person descriptions, progressive disclosure)

⚠️ Action Required: Close and reopen the project.
The IDE needs to re-scan to discover the new slash commands.

Next step:
→ Close and reopen the project
→ Then run /status to see where things stand
```
