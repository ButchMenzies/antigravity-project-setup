---
description: Verify the Antigravity setup system is internally consistent — character counts, cross-references, scenario traces
---

# Critical Analysis

Run this after making any changes to the Antigravity Project Setup files. Verifies that the entire setup chain works correctly across all scenarios.

> **This workflow is specific to the Antigravity Project Setup repo.** It is NOT installed into user projects.

---

## Step 1: Character Counts

Every workflow must be under **12,000 characters**.

```bash
wc -c .agent/workflows/*.md | sort -n
```

Also check templates and setup guide:
```bash
wc -c templates/*.md AGENT_SETUP_GUIDE.md BOOTSTRAP.md
```

Flag any file over 12,000 chars. If over, identify trim targets — look for:
- Inline content that could reference a template instead
- Verbose instructions that can be condensed
- Duplicated content across files
- Example blocks that could be shortened

---

## Step 2: Available Commands ↔ Installed Workflows

The Available Commands lists in the AGENT.md templates must match exactly what gets installed.

### 2a. Extract commands from both templates

```bash
echo "--- AGENT-code.md ---"
grep "^- \`/" templates/AGENT-code.md
echo ""
echo "--- AGENT-workspace.md ---"
grep "^- \`/" templates/AGENT-workspace.md
```

### 2b. List what actually gets installed

These workflows are **excluded** from user projects (removed after copy):
- `setup.md` — only used during initial onboarding
- `new-project.md` — only used from setup guide for empty folders
- `update-guide.md` — meta-workflow for maintaining this repo
- `critical-analysis.md` — meta-workflow for verifying this repo

Everything else should be listed in Available Commands.

### 2c. Cross-check both directions

Run these programmatically, not just mentally:

```bash
# Every command → has a workflow?
grep "^- \`/" templates/AGENT-code.md | sed 's/.*`\/\([^`]*\)`.*/\1/' | while read cmd; do
  if [ -f ".agent/workflows/${cmd}.md" ]; then echo "  ✅ /${cmd}"; else echo "  ❌ /${cmd} — NO WORKFLOW"; fi
done
```

```bash
# Every installed workflow → has a command entry?
ls .agent/workflows/*.md | while read f; do
  b=$(basename "$f" .md)
  case "$b" in setup|new-project|update-guide|critical-analysis) continue;; esac
  if grep -q "\`/${b}\`" templates/AGENT-code.md; then echo "  ✅ ${b}.md"; else echo "  ❌ ${b}.md — NOT in Available Commands"; fi
done
```

- No excluded workflow should appear in Available Commands.

### 2d. Template sync

`templates/AGENT.md` must be identical to `templates/AGENT-code.md`:

```bash
diff templates/AGENT.md templates/AGENT-code.md
```

If they differ, copy AGENT-code.md over AGENT.md.

---

## Step 3: Skill Install Parity

Both install paths must install the **same set of skills**.

### 3a. Verify skill directories exist in the repo

Every skill referenced in the install commands must actually exist:

```bash
for skill in planning ux-design offer-strategy lead-strategy voice-notes-triage; do
  if [ -f "skills/${skill}/SKILL.md" ]; then echo "  ✅ skills/${skill}/SKILL.md"; else echo "  ❌ skills/${skill}/SKILL.md MISSING"; fi
done
# visual-qa lives in .agent/skills/ not skills/
if [ -f ".agent/skills/visual-qa/SKILL.md" ]; then echo "  ✅ .agent/skills/visual-qa/SKILL.md"; else echo "  ❌ .agent/skills/visual-qa/SKILL.md MISSING"; fi
```

### 3b. Compare install commands

Extract and compare the `mkdir -p` and `cp` commands from:
1. `AGENT_SETUP_GUIDE.md` Step 2 + Step 3B
2. `.agent/workflows/new-project.md` Phase 2 Step 2

They should create the same skill directories and copy the same SKILL.md files.

Current standard set: planning, ux-design, offer-strategy, lead-strategy, voice-notes-triage, visual-qa

---

## Step 4: No Hardcoded Paths

Search for device-specific paths that will break on other machines:

```bash
grep -rn "/Users/" AGENT_SETUP_GUIDE.md .agent/workflows/*.md templates/*.md
grep -rn "~/Desktop" AGENT_SETUP_GUIDE.md .agent/workflows/*.md templates/*.md
grep -rn "/masonridge\|/butchmenzies" AGENT_SETUP_GUIDE.md .agent/workflows/*.md templates/*.md
```

The only acceptable paths are:
- `.agent/` relative paths
- `/tmp/ag-setup` (temporary clone directory)
- GitHub raw URLs (`https://raw.githubusercontent.com/ButchMenzies/...`)
- GitHub repo URLs (`https://github.com/ButchMenzies/...`)

Flag anything else — it will break on other users' machines.

---

## Step 5: No && Chaining in Bash Blocks

Core Rule 7 prohibits `&&` chaining. Our own setup instructions must follow this rule.

Check specifically inside bash code blocks (not in prose):

```bash
grep -n "&&" AGENT_SETUP_GUIDE.md .agent/workflows/*.md templates/*.md
```

For each match, check whether it's inside a ```bash block or in prose text. Only flag matches inside bash blocks.

Acceptable exceptions:
- `&&` in prose descriptions, markdown text, or comments
- Inside template content that will be generated (e.g., git commands the agent writes into files)

If found in bash blocks, rewrite using `if/then/fi` or separate commands.

---

## Step 6: Template Content Verification

### 6a. Core rules structure

Both templates must have these three sections at the top:
1. `## ⚠️ New Chat? Start Here — MANDATORY`
2. `## ⚠️ Core Rules (Always Apply)`
3. `## Available Commands`

```bash
for f in templates/AGENT-code.md templates/AGENT-workspace.md; do
  echo "=== $(basename $f) ==="
  grep "^## " "$f"
  echo ""
done
```

### 6b. Code template must have

- Rules 1–9 (including terminal discipline, browser, dev server port)
- Project Overview, Tech Stack, Architecture, Conventions, Local Development sections
- Resources section with links to skills, catalog, memory, global reference

### 6c. Workspace template must have

- Rules 1–8 (including file organization, research discipline)
- Project Overview, Goals, Audience / Target Market, Key Resources sections
- Resources section
- NO terminal discipline rules (7–9 from code template)
- NO Tech Stack or Local Development sections

### 6d. memory.md template

Verify `templates/memory.md` exists and has the expected sections:

```bash
grep "^## " templates/memory.md
```

Must have: Key Decisions, Lessons Learned, User Preferences, Session Log

---

## Step 7: Version Number Consistency

The version number appears in multiple places and must match:

```bash
echo "Setup guide header:"
grep "Latest version" AGENT_SETUP_GUIDE.md
echo ""
echo "Version written to file:"
grep 'echo.*> .agent/version' AGENT_SETUP_GUIDE.md
echo ""
echo "Version in completion message:"
grep "installed/updated to" AGENT_SETUP_GUIDE.md
echo ""
echo "Version in 'already current' check:"
grep "version =" AGENT_SETUP_GUIDE.md | head -1
echo ""
echo "Memory log template version:"
grep "updated to v" AGENT_SETUP_GUIDE.md
```

All five must reference the same version number.

### 7b. Version bump check

If any setup system files have been modified since the last commit tagged with a version bump, the version **must** be incremented. Check:

1. **Version number** — bumped from previous (e.g., v5 → v6)
2. **Date** — header date matches today's date (when the version was bumped)
3. **Memory log template** — the `**Changes**:` description accurately describes what changed in this version, not the previous version's changes
4. **Completion message** — new features or changes listed match actual changes

---

## Step 8: GitHub URL Verification

Verify all GitHub URLs referenced in the setup chain are correct and point to files that exist:

```bash
grep -rn "raw.githubusercontent.com\|github.com/ButchMenzies" AGENT_SETUP_GUIDE.md .agent/workflows/setup.md .agent/workflows/new-project.md
```

For each URL found, verify:
- The repo name is correct: `ButchMenzies/antigravity-project-setup`
- The branch is `main`
- The file path matches an actual file in the repo

Key URLs to verify:
- `AGENT_SETUP_GUIDE.md` (referenced from BOOTSTRAP.md)
- `new-project.md` (referenced from setup guide Step 1)
- `templates/AGENT-code.md` (referenced from setup guide Step 4 and new-project Phase 3)
- `templates/AGENT-workspace.md` (referenced from setup guide Step 4 and new-project Phase 3)
- `skills/README.md` (referenced from setup.md and create-skill.md)

---

## Step 9: Scenario Traces (Concrete Verification)

For each scenario, actually read the relevant files and verify the branching logic. Do NOT just mentally skim — read the specific lines.

### Scenario A: Fresh install (project has source files)

1. Read `BOOTSTRAP.md` — verify it points to the correct setup guide URL
2. Read `AGENT_SETUP_GUIDE.md` Step 1 — verify the "FRESH PROJECT" detection logic (`ls *.* src/ app/ lib/ public/`)
3. Verify Step 2 creates all required directories
4. Verify Step 3B copies workflows, removes excluded ones, copies all 6 skills
5. Verify Step 3C only copies templates if they don't exist (if/then/fi guards)
6. Verify Step 4 skips for fresh install ("just created → Skip to Step 5")
7. After reopen, user runs `/setup` — read `setup.md` and verify:
   - Pre-flight detection works
   - Q&A covers project type (Section 2)
   - File Generation references correct templates with correct URLs or paths
   - setup.md references `templates/memory.md` for memory creation

### Scenario B: Empty folder → code project

1. Read `AGENT_SETUP_GUIDE.md` Step 1 — verify empty folder detection redirects to new-project.md URL
2. Read `new-project.md` — verify Phase 1 Q1 options 1-6 lead to scaffold path
3. Verify Phase 2 installs all workflows AND all 6 skills (matching setup guide)
4. Verify Phase 2 removes excluded workflows
5. Verify Phase 3 fetches `AGENT-code.md` template (URL must be correct and complete)
6. Verify Phase 3 references `templates/memory.md`
7. Verify completion message is accurate

### Scenario C: Empty folder → workspace project

1. Verify Q1 option 7 leads to Phase 1W
2. Verify Phase 1W creates `docs/ research/ output/ references/` structure
3. Verify Phase 3 fetches `AGENT-workspace.md` template (URL must be correct)
4. Verify no dev port or scaffolding steps run
5. Verify Phase 3 conductor section handles workspace differently (`strategy.md` not `tech-stack.md`)

### Scenario D: Update (existing project, older version)

1. Read Step 1 — verify version detection catches all old versions (1, 2, 3, 4, or "none" with AGENT.md)
2. Verify Step 3B overwrites workflows and skills (no guards — correct for updates)
3. Verify Step 3C does NOT overwrite existing AGENT.md, memory.md (if/then/fi guards)
4. Read Step 4 — verify the core rules merge logic:
   - Correctly detects project type from existing AGENT.md content
   - Fetches correct template URL
   - Strips old sections by header name matching
   - Inserts new sections at top
   - Preserves all existing project data below
5. Verify Step 6 version, log message, and completion message are all consistent

### Scenario E: Already current

1. Verify Step 1 detects current version → reads files → exits immediately
2. Verify no files are overwritten, no directories created

---

## Step 10: Excluded Workflow List Consistency

The `rm -f` line appears in two places. They must list the same files:

```bash
grep "rm -f" AGENT_SETUP_GUIDE.md .agent/workflows/new-project.md
```

Both must exclude the same set of workflows. Verify they match exactly.

---

## Report

After running all checks, summarize:

```
✅ Critical Analysis Complete

Character counts: [all under 12K / X files over]
Commands ↔ Workflows: [all matched / X mismatches]
Template sync: [AGENT.md = AGENT-code.md / differs]
Skill parity: [matched / X differences]
Skill files exist: [all present / X missing]
Hardcoded paths: [none found / X found]
&& chaining: [none in bash blocks / X found]
Template structure: [correct / X issues]
memory.md template: [correct / X issues]
Version consistency: [all match / X differ]
GitHub URLs: [all valid / X broken]
Scenarios: [all pass / X issues]
Completion messages: [accurate / X inaccurate]
Excluded list: [consistent / differs]

[List any issues found with recommended fixes]
```
