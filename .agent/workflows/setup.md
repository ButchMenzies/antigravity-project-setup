---
description: Interactive project onboarding — scans codebase, gathers context, creates project files
---

# Project Setup

Interactive onboarding for a new Antigravity project. Creates `AGENT.md`, `memory.md`, discovers skills, and optionally sets up Conductor artifacts.

## Pre-flight

1. Verify `.agent/workflows/` exists (bootstrapper already ran)
2. Check if `AGENT.md` already has **real project data** (not just template placeholders).
   A file is still a template if it contains placeholder text like *"Populate this section during project onboarding"*. A file has real data if `## Project Overview` contains actual project information.
   - If real data found → Ask user: **Re-run setup** (overwrite AGENT.md, preserve memory.md) or **Skip** (read existing files, proceed with request)?
   - If user chooses skip: read AGENT.md and memory.md, then proceed with the user's request
3. Check `.gitignore` — if it contains `.agent/` (blanket ignore), remove that line and replace with selective ignores:
   ```
   .agent/memory.md
   .agent/memory-archive.md
   .agent/pending-skill-uploads.md
   .agent/current-plan.md
   ```
4. Detect project type:
   ```bash
   ls -la
   ls package.json pyproject.toml requirements.txt go.mod Cargo.toml 2>/dev/null
   cat README.md 2>/dev/null | head -30
   ```

## Interactive Q&A Protocol

**Rules:** Ask ONE question per turn. Wait for response. Offer 2–3 suggested answers plus "Type your own". Max 5 questions per section. Pre-fill answers you can infer from the codebase.

---

### Section 1: Project Basics (3 questions)

**Q1: Project Name** — Infer from directory name or package.json. Ask to confirm.

**Q2: Project Description** — "In one sentence, what does this project do?" Suggest from README or package.json if available.

**Q3: Project Stage** — Brand new / Early development / Active development / Maintenance

### Section 2: Project Type

**Q4: Project Type** — Based on pre-flight detection, suggest the most likely type:

1. **Code / App** — building software (web, mobile, API, CLI, library)
2. **Strategy / Planning** — brainstorming, marketing, business planning, consulting
3. **Content / Writing** — courses, copy, documentation, email sequences
4. **Research / Analysis** — market research, data analysis, competitive analysis
5. **Hybrid** — some code, some planning/content

This determines which sections follow and which AGENT.md template is used.

### Section 3: Tech Stack (code/hybrid projects only)

Skip entirely for strategy/content/research projects.

Scan the codebase first:
```bash
cat package.json 2>/dev/null | head -30
cat requirements.txt 2>/dev/null | head -20
find . -maxdepth 2 -name "*.ts" -o -name "*.tsx" -o -name "*.py" -o -name "*.go" 2>/dev/null | head -10
```

**Q5: Confirm Tech Stack** — Present detected language, framework, database, key dependencies. Ask if correct.

**Q6: Development Port** — 5010 (Antigravity default) / Detected from scripts / Type your own

### Section 4: Work Patterns (all project types, 2 questions)

**Q7: Primary Work Type**
- Code projects: Building features / Bug fixes / Refactoring / Mix
- Non-code projects: Creating new content / Iterating existing work / Research / Mix

**Q8: Work Style** — Plan first / Just go / Depends on size

### Section 5: Skill Discovery

Check the Antigravity skills library for matching skills:
`https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/skills/README.md`

Also check the global reference library and Superpowers library for additional matches:
- `https://raw.githubusercontent.com/sickn33/antigravity-awesome-skills/main/skills_index.json`
- `https://raw.githubusercontent.com/obra/superpowers/main/README.md`

Present combined findings in a table. Ask: Install all recommended / Let me choose / Skip for now.

If user agrees, copy from Antigravity library into `.agent/skills/` or run `/create-skill` for global library skills.

### Section 6: UX Design Foundation (code/hybrid projects with UI only)

Skip for non-code projects and code projects without user-facing UI.

Ask: "Would you like to establish a design direction? This covers personas, brand voice, color, typography."
- Yes → run `/ux-design` inline
- Later → note in memory
- Skip → no UI

### Section 7: MCP Detection (code/hybrid projects only)

Skip for non-code projects.

```bash
cat package.json 2>/dev/null | grep -iE "supabase|stripe|firebase|mongodb|postgres|@octokit|slack"
cat requirements.txt 2>/dev/null | grep -iE "supabase|stripe|firebase|pymongo|psycopg|slack"
```

If services detected, present them with recommended MCPs. Ask if user wants to set any up.

### Section 8: Project Scope (all project types)

"Is this a long-term project or a quick experiment?"
1. **Long-term** — full project management (tracks, specs, plans) → create `conductor/` artifacts
2. **Medium** — basics, nothing heavy → skip conductor
3. **Quick experiment** — minimal → skip conductor

---

## File Generation

After Q&A, generate these files:

### AGENT.md

Use the appropriate template based on project type:
- **Code/Hybrid projects** → Read `templates/AGENT-code.md` from the Antigravity repo (or use the locally installed copy)
- **Strategy/Content/Research** → Read `templates/AGENT-workspace.md`

Populate all placeholder sections with real answers from the Q&A. Replace the title "Project Configuration" with the actual project name. For code projects, fill in Tech Stack, Local Development, Architecture, and Conventions. For non-code projects, fill in Goals, Audience, and Key Resources.

### memory.md

If `.agent/memory.md` already exists → **do NOT overwrite it**.

Only create if not present. Use `templates/memory.md` from the Antigravity repo as the base. Set the work style preference from Q8 and add today's date to the session log.

---

## Completion

```
✅ Setup complete!

Created:
- .agent/AGENT.md — project context and core rules
- .agent/memory.md — session history
- .agent/skills-catalog.md — skill references
- [list any skills created]
- [list conductor/ artifacts if created]

Next steps:
→ /new-track — to start your first piece of work
→ /status — to see project overview
```
