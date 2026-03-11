# Implementation Plan: Workflow Categories & v10

## Spec Summary

Split workflows into Essential (11, always installed) and Additional (5, offered as options). Create a dedicated `update.md` workflow for the update path. Simplify `AGENT_SETUP_GUIDE.md` to a pure router. Bump version to v10. During updates, check for existing additional workflows and ask user if they want to keep/remove them.

## Acceptance Criteria

- [ ] Essential workflows always installed without asking
- [ ] Additional workflows offered during setup/update with descriptions
- [ ] `update.md` workflow exists and handles the full update path
- [ ] `AGENT_SETUP_GUIDE.md` is a pure router (detect → route)
- [ ] `new-project.md` uses same essential/additional split
- [ ] During update, existing additional workflows checked — user asked to keep/remove
- [ ] Available Commands reflects the essential/additional split in templates
- [ ] Version bumped to v10 with accurate change log
- [ ] Update path from any old version (v1–v9) works correctly
- [ ] Critical analysis passes

---

## Phase 1: Create `update.md` Workflow

### Tasks

- [ ] Task 1.1: Create `.agent/workflows/update.md`
  - Step 1: Clone repo to /tmp/ag-setup
  - Step 2: Install essential workflows (overwrite existing)
  - Step 3: Install skills (overwrite existing, skip bundles)
  - Step 4: Check for existing additional workflows — if any are installed, ask user: keep or remove each?
  - Step 5: Offer additional workflows not currently installed — show descriptions, let user pick
  - Step 6: Copy templates only if they don't exist (AGENT.md, memory.md, skills-catalog.md, USER_GUIDE.md)
  - Step 7: Merge core rules into existing AGENT.md (detect project type → fetch template → strip old headers → insert new at top)
  - Step 8: Clean up /tmp/ag-setup
  - Step 9: Configure .gitignore
  - Step 10: Write version file, add session log entry, show completion message
  - Must be excluded from user installs (added to rm -f list)

- [ ] Task 1.2: Add `update.md` to the excluded workflows list
  - In AGENT_SETUP_GUIDE.md rm -f line
  - In new-project.md rm -f line

### Verification

- [ ] Read update.md — all steps present
- [ ] Under 12K characters
- [ ] Excluded from both install paths

---

## Phase 2: Simplify `AGENT_SETUP_GUIDE.md` + Version Bump

### Tasks

- [ ] Task 2.1: Rewrite AGENT_SETUP_GUIDE.md as a pure router
  - Step 1: Detect state (same detection logic)
  - If already current (v10) → read files, done
  - If needs update (v1–v9) → fetch and follow update.md from GitHub
  - If fresh with source files → Steps 2–5 (basic bootstrap with essential-only install + additional offer)
  - If empty folder → fetch and follow new-project.md from GitHub

- [ ] Task 2.2: Version bump all references to v10
  - Header version
  - Version detection check
  - echo to .agent/version
  - "already current" check
  - Change log message
  - Completion message

- [ ] Task 2.3: Update change log and completion message
  - Mention living documents: /audit, enhanced /end-session, enhanced /status, product.md/tech-stack.md templates
  - Mention workflow categories: essential vs additional
  - Mention update.md workflow

### Verification

- [ ] Setup guide is significantly shorter (router + fresh bootstrap only)
- [ ] All 5 version references say v10
- [ ] Under 12K characters

---

## Phase 3: Update `new-project.md` with Categories

### Tasks

- [ ] Task 3.1: Update Phase 2 workflow install to use essential/additional split
  - Install essential workflows only by default
  - Remove excluded workflows
  - Ask user which additional workflows they want with descriptions
  - Copy selected additional workflows

### Verification

- [ ] Essential/additional split matches update.md
- [ ] Under 12K characters

---

## Phase 4: Update Available Commands in Templates

### Tasks

- [ ] Task 4.1: Split Available Commands into Essential and Additional sections in all templates
  - `templates/AGENT-code.md` — Essential Commands section, then Additional Commands section
  - `templates/AGENT-workspace.md` — same
  - `templates/AGENT.md` — keep in sync with AGENT-code.md
  - `.agent/AGENT.md` — update this project (has all commands since it's the source repo)

### Verification

- [ ] All templates consistent
- [ ] `diff templates/AGENT.md templates/AGENT-code.md` returns nothing

---

## Phase 5: Critical Analysis

- [ ] Run `/critical-analysis` and fix any issues found

---

## Final Verification

- [ ] All acceptance criteria met
- [ ] All files under 12K characters
- [ ] Memory updated
