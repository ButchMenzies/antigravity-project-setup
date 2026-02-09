---
description: Interactive project onboarding — scans codebase, gathers context, creates project files
---

# Project Setup

Interactive onboarding for a new Antigravity project. Creates `AGENT.md`, `memory.md`, discovers skills, and optionally sets up Conductor artifacts.

## Pre-flight

1. Verify `.agent/workflows/` exists (bootstrapper already ran)
2. Check if `AGENT.md` already has **real project data** (not just template placeholders).
   A file is still a template if it contains placeholder text like *"Run `/setup` to populate this section"* or *"Project Configuration"* as the title. A file has real data if `## Project Overview` contains actual project information.
   - If real data found: Ask user:
     ```
     AGENT.md already has project data. What would you like to do?
     
     1. Re-run setup — I'll update AGENT.md with fresh Q&A answers (memory.md will be preserved)
     2. Skip setup — just read the existing files and start working
     ```
   - If user chooses re-run: proceed with Q&A, overwrite AGENT.md, but **never overwrite memory.md**
   - If user chooses skip: read AGENT.md and memory.md, then proceed with the user's request
3. Check `.gitignore` — if it contains `.agent/` (blanket ignore), remove that line and replace with selective ignores:
   ```
   .agent/memory.md
   .agent/memory-archive.md
   .agent/pending-skill-uploads.md
   .agent/current-plan.md
   ```
   **Never blanket-ignore `.agent/`** — it breaks slash command autocomplete.
4. Detect project type:

```bash
ls -la
ls package.json pyproject.toml requirements.txt go.mod Cargo.toml 2>/dev/null
cat README.md 2>/dev/null | head -30
cat Replit.md 2>/dev/null | head -50
```

## Interactive Q&A Protocol

**CRITICAL RULES:**
- Ask **ONE question per turn**
- Wait for user response before proceeding
- Offer 2-3 suggested answers plus "Type your own"
- Maximum 5 questions per section
- If you can infer the answer from codebase analysis, pre-fill and ask to confirm

---

### Section 1: Project Basics (max 3 questions)

**Q1: Project Name**

```
What is your project called?

Suggested:
1. [Infer from directory name]
2. [Infer from package.json name field]
3. Type your own
```

**Q2: Project Description**

```
In one sentence, what does this project do?

Suggested:
1. [Infer from README.md or package.json description]
2. Type your own
```

**Q3: Project Stage**

```
What stage is this project at?

1. Brand new — haven't started coding yet
2. Early development — basic structure in place
3. Active development — core features working
4. Maintenance — mostly bug fixes and improvements
```

### Section 2: Tech Stack (max 3 questions)

Before asking, scan the codebase:

```bash
# Detect languages and frameworks
cat package.json 2>/dev/null | head -30
cat requirements.txt 2>/dev/null | head -20
ls src/ app/ lib/ 2>/dev/null
find . -maxdepth 2 -name "*.ts" -o -name "*.tsx" -o -name "*.py" -o -name "*.go" 2>/dev/null | head -10
```

**Q1: Confirm Tech Stack**

```
I detected the following tech stack:

- Language: [detected]
- Framework: [detected]
- Database: [detected or "none detected"]
- Key dependencies: [top 5 from package.json/requirements.txt]

Is this correct? Anything to add or change?

1. Looks correct
2. Let me adjust: [type changes]
```

**Q2: Development Port**

```
What port should the dev server run on?

1. 5010 (Antigravity default)
2. [Detected from package.json scripts]
3. Type your own
```

### Section 3: Work Patterns (max 2 questions)

**Q1: Primary Work Type**

```
What kind of work will you mainly be doing?

1. Building new features
2. Bug fixes and maintenance
3. Refactoring / improving existing code
4. Mix of everything
```

**Q2: Work Style**

```
How do you prefer to work?

1. Plan first, then implement (I'll review plans before you code)
2. Just go — implement directly, I'll review as we go
3. Depends on the task size
```

### Section 4: Skill Discovery

Based on detected tech stack and work patterns, search for existing skills:

**Step 1: Check the Antigravity skills library (curated, tested skills):**
Read: `https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/skills/README.md`

Look for skills that match the detected tech stack.

**Step 2: Check the global reference library (for inspiration):**
```bash
curl -s https://raw.githubusercontent.com/sickn33/antigravity-awesome-skills/main/skills_index.json | grep -i "<framework>" | head -5
curl -s https://raw.githubusercontent.com/sickn33/antigravity-awesome-skills/main/skills_index.json | grep -i "<language>" | head -5
```

**Present findings:**

```
Based on your tech stack, I found these skills:

From Antigravity library (ready to copy):
| Skill | What it does |
|-------|--------------|
| `<skill>` | <description> |

From global library (can create local version):
| Skill | What it does |
|-------|--------------|
| `<skill>` | <description> |

Shall I install the Antigravity skills and/or create local versions of any global skills?

1. Yes, install all recommended
2. Let me choose which ones
3. Skip for now — I'll create skills as needed
```

If user agrees:
- For Antigravity library skills: copy directly from the repo into `.agent/skills/`
- For global library skills: run `/create-skill` using the global version as reference

### Section 5: MCP Detection

```bash
# Scan for MCP-compatible services
cat package.json 2>/dev/null | grep -iE "supabase|stripe|firebase|mongodb|postgres|@octokit|slack"
cat requirements.txt 2>/dev/null | grep -iE "supabase|stripe|firebase|pymongo|psycopg|slack"
```

If services detected:

```
I detected these services that could benefit from MCP integration:

| Service | Recommended MCP | Benefit |
|---------|-----------------|---------|
| [detected] | [MCP name] | [benefit] |

MCPs give me direct access to these services. Want to set any up?

1. Yes, let's set up [most relevant]
2. Skip for now
```

### Section 6: Project Scope

```
One last question: is this a project you'll maintain long-term, or a quick experiment?

1. Long-term — I want full project management (tracks, specs, plans)
2. Medium — I want basics but nothing too heavy
3. Quick experiment — keep it minimal
```

If long-term: Create `conductor/` artifacts (product.md, tech-stack.md, workflow.md, tracks.md).
If medium/quick: Skip conductor, rely on slash commands for workflow.

---

## File Generation

After Q&A, generate these files:

### AGENT.md

Populate with real answers (not placeholders):

```markdown
# [Project Name]

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

---

## Project Overview
[From Q&A answers]

## Tech Stack
[From Q&A answers + detection]

## Architecture
[From codebase scan]

## Conventions
[From detected patterns or user input]

---

## Platform Notes
[If Replit.md exists, extract and summarize key info here:
- Run command (from .replit or Replit.md)
- Environment variables mentioned
- Deployment configuration
- Any Replit-specific setup notes
Do NOT modify or delete Replit.md — the project may be re-opened in Replit later.]

## Local Development

- **Port**: [from Q&A]
- **Start command**: [detected]
- **Key scripts**: [from package.json]

---

## Resources

- **Local Skills**: `.agent/skills/`
- **Skills Catalog**: `.agent/skills-catalog.md`
- **Memory**: `.agent/memory.md`
- **Global Skills Reference**: https://github.com/sickn33/antigravity-awesome-skills
```

### memory.md

If `.agent/memory.md` already exists → **do NOT overwrite it**. Preserve all existing session history.

Only create if the file does not exist:

```markdown
# Project Memory

> Agent-maintained record of decisions, lessons, and context.
> Update after completing features, making decisions, or solving problems.
>
> **Maintenance:** When this file exceeds ~100 entries or becomes hard to scan,
> archive older entries to `.agent/memory-archive.md` and keep only the last
> 30 days of entries here. Always keep User Preferences — those never expire.

---

## Key Decisions

<!-- Append new decisions here using this format:
### YYYY-MM-DD [Title]
**Context**: [what prompted the decision]
**Decision**: [what was decided]
**Rationale**: [why this choice]
-->

## Lessons Learned

<!-- Append new lessons here using this format:
### YYYY-MM-DD [Title]
**Problem**: [what went wrong]
**Fix**: [how it was resolved]
**Prevention**: [how to avoid next time]
-->

## User Preferences

- Work style: [from Q&A]

<!-- Append new preferences as bullet points -->

## Session Log

| Date | Summary |
|------|---------|
| [today] | Initial project setup via /setup |
```

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
