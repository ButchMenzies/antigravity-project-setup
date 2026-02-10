---
description: Scaffold a brand new project — choose what to build, create folder structure, install dependencies
---

# New Project

Scaffold a new project from a blank folder. This workflow handles **everything** for blank projects: scaffolding, Antigravity bootstrap, and project onboarding. **No need to run `/setup` afterward.**

> This workflow is triggered by the setup guide when it detects a completely empty project directory. At this point, `.agent/` does NOT exist — the folder is empty.

---

## Phase 1: Plan & Scaffold

### Pre-flight

Read the planning skill from GitHub and apply its principles throughout:
`https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/skills/planning/SKILL.md`

### Project Type (Q1)

```
What are you building?

1. Web app — browser-based application (or full-stack with API)
2. API / backend — server-side service or REST/GraphQL API
3. Mobile app — iOS, Android, or cross-platform
4. CLI tool — command-line utility
5. Library / package — reusable module
6. Something else — describe it
```

**Wait for user response before proceeding.**

### Description (Q2)

```
Describe what it should do in one or two sentences.
```

**Wait for user response before proceeding.**

### Tech Stack (Q3)

Based on the project type, **recommend a default stack** with a brief explanation. The user can accept or change it.

#### Web App / Full-Stack

```
For a web app, I'd recommend:

→ Next.js with TypeScript (App Router)

This gives you:
- Server-side rendering and static generation
- Built-in API routes (no separate backend)
- Works well with Supabase, Stripe, and other services
- Largest ecosystem and community

Happy with this?

1. Sounds good — let's go with Next.js
2. I want something lighter (Vite + React — pure client-side SPA)
3. I want no framework (vanilla HTML/CSS/JS)
4. I have something else in mind
```

#### API / Backend

```
For an API, I'd recommend:

→ Express with TypeScript

This gives you:
- Most widely supported Node.js framework
- Huge middleware ecosystem
- Easy to extend with auth, validation, etc.

Happy with this?

1. Sounds good — let's go with Express
2. I want something faster (Fastify)
3. I'd prefer Python (FastAPI)
4. I have something else in mind
```

#### Mobile App

```
For a mobile app, I'd recommend:

→ React Native with Expo (TypeScript)

This gives you:
- Cross-platform (iOS + Android from one codebase)
- JavaScript ecosystem (same as web)
- Expo handles the painful native tooling

Happy with this?

1. Sounds good — let's go with React Native + Expo
2. I'd prefer Flutter (Dart)
3. I want native iOS only (Swift)
4. I want native Android only (Kotlin)
5. I have something else in mind
```

#### CLI Tool

```
For a CLI tool, I'd recommend:

→ Node.js with TypeScript

This gives you:
- Fast to build, easy to distribute via npm
- Great CLI libraries (commander, inquirer)

Happy with this?

1. Sounds good — Node.js + TypeScript
2. I want a compiled binary (Go)
3. I want a compiled binary (Rust)
4. I'd prefer Python
5. I have something else in mind
```

#### Library / Package

```
For a library, I'd recommend:

→ TypeScript npm package

This gives you:
- Type-safe API for consumers
- Easy publishing to npm

Happy with this?

1. Sounds good — TypeScript npm package
2. I'd prefer a Python package (PyPI)
3. I'd prefer a Go module
4. I have something else in mind
```

**Wait for user response before proceeding.**

### Scaffold

Follow these rules:

- **Use the standard tooling.** Don't hand-roll folder structures when a CLI scaffolder exists. Use `npx create-*`, `npm init`, `cargo init`, etc.
- **Non-interactive mode.** Run scaffolders with flags that avoid interactive prompts
- **Check the `--help` flag first.** Before running any scaffolder, run it with `--help` to see available options. Don't guess at flags
- **Install in current directory.** Always scaffold into `./`, not a subdirectory
- **TypeScript by default.** Unless the user specifically chose JavaScript
- **Git init.** If the scaffolder doesn't create a git repo, run `git init`

#### Framework-Specific Notes

**Next.js:**
```bash
npx -y create-next-app@latest ./ --help  # Check available flags first
npx -y create-next-app@latest ./ --ts --eslint --app --src-dir --import-alias "@/*"
```

**Vite + React:**
```bash
npx -y create-vite@latest ./ --help
npx -y create-vite@latest ./ --template react-ts
npm install
```

**Express:**
```bash
npm init -y
npm install express
npm install -D typescript @types/express @types/node tsx
npx tsc --init
mkdir -p src
# Create src/index.ts with basic Express server
```

**React Native + Expo:**
```bash
npx -y create-expo-app@latest ./ --help
npx -y create-expo-app@latest ./ --template blank-typescript
```

#### Additional Folders

After the scaffolder runs, create these folders if they don't already exist:

```bash
mkdir -p docs/design          # UI mockups, screenshots, design exports
mkdir -p public/assets         # Logo, favicon, images (web projects)
```

For web/full-stack projects, ensure there is a dedicated styles directory (e.g., `src/styles/`) for CSS variables and design tokens.

#### Verify

```bash
npm run dev  # or equivalent
```

If the dev server starts, kill it after confirming. If it fails, troubleshoot before proceeding.

---

## Phase 2: Bootstrap Antigravity

Now that the project has source files, install the Antigravity system.

### Create Directory Structure

```bash
mkdir -p .agent/workflows
mkdir -p .agent/skills/create-skill
mkdir -p .agent/skills/planning
```

### Install Workflows

Read each file from the GitHub repo and create it locally:

**Raw base:** `https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main`

| Remote Path | Local Path |
|------------|------------|
| `.agent/workflows/setup.md` | `.agent/workflows/setup.md` |
| `.agent/workflows/new-track.md` | `.agent/workflows/new-track.md` |
| `.agent/workflows/edit.md` | `.agent/workflows/edit.md` |
| `.agent/workflows/implement.md` | `.agent/workflows/implement.md` |
| `.agent/workflows/status.md` | `.agent/workflows/status.md` |
| `.agent/workflows/update-memory.md` | `.agent/workflows/update-memory.md` |
| `.agent/workflows/end-session.md` | `.agent/workflows/end-session.md` |
| `.agent/workflows/create-skill.md` | `.agent/workflows/create-skill.md` |
| `.agent/workflows/new-project.md` | `.agent/workflows/new-project.md` |

For each file: read `{raw base}/{remote path}` → write content to local path.

### Install Skills

| Remote Path | Local Path |
|------------|------------|
| `.agent/skills/create-skill/SKILL.md` | `.agent/skills/create-skill/SKILL.md` |
| `skills/planning/SKILL.md` | `.agent/skills/planning/SKILL.md` |

### Install Templates

Read and create locally:

| Remote Path | Local Path |
|------------|------------|
| `templates/skills-catalog.md` | `.agent/skills-catalog.md` |
| `templates/USER_GUIDE.md` | `.agent/USER_GUIDE.md` |

**Do NOT install `templates/AGENT.md` or `templates/memory.md`** — Phase 3 creates these with real project data.

### Configure .gitignore

The scaffolder likely created a `.gitignore`. **Append** these Antigravity-specific ignores to the existing file (do not replace existing content):

```
# Antigravity — keep workflows/skills tracked, ignore personal data
.agent/memory.md
.agent/memory-archive.md
.agent/pending-skill-uploads.md
.agent/current-plan.md
```

**Important:** If the `.gitignore` contains `.agent/` as a blanket ignore, remove that line and keep only the selective ignores above. Blanket-ignoring `.agent/` breaks slash command autocomplete.

---

## Phase 3: Project Onboarding

We already know the project type, description, and tech stack from Phase 1. Gather the remaining context.

### Work Style (Q4)

```
How do you prefer to work?

1. Plan first, then implement (I'll review plans before you code)
2. Just go — implement directly, I'll review as we go
3. Depends on the task size
```

**Wait for user response before proceeding.**

### Project Scope (Q5)

```
Is this a project you'll maintain long-term, or a quick experiment?

1. Long-term — I want full project management (tracks, specs, plans)
2. Medium — I want basics but nothing too heavy
3. Quick experiment — keep it minimal
```

**Wait for user response before proceeding.**

If long-term: Create `conductor/` artifacts (product.md, tech-stack.md, workflow.md, tracks.md).
If medium/quick: Skip conductor, rely on slash commands for workflow.

### Generate AGENT.md

Create `.agent/AGENT.md` with real answers (not placeholders):

```markdown
# [Project Name — from directory name or Q2]

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
- `/setup` — interactive project onboarding
- `/new-track` — plan a new piece of work
- `/edit` — revise a plan before implementing
- `/implement` — execute a plan with progress tracking
- `/status` — show project status
- `/update-memory` — log a decision, lesson, or preference
- `/end-session` — wrap up the current session
- `/create-skill` — create a reusable local skill
- `/new-project` — scaffold a blank project (choose framework, create structure)

---

## Project Overview
[From Q2 — project description]

## Tech Stack
[From Q3 — framework, language, key dependencies from package.json]

## Local Development

- **Port**: [detected from scaffold]
- **Start command**: [detected from scaffold, e.g., npm run dev]
- **Key scripts**: [from package.json scripts section]

---

## Resources

- **Local Skills**: `.agent/skills/`
- **Skills Catalog**: `.agent/skills-catalog.md`
- **Memory**: `.agent/memory.md`
- **Global Skills Reference**: https://github.com/sickn33/antigravity-awesome-skills
```

### Generate memory.md

Create `.agent/memory.md`:

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

- Work style: [from Q4]

<!-- Append new preferences as bullet points -->

## Session Log

| Date | Summary |
|------|---------|
| [today] | Project scaffolded and set up via /new-project |
```

### Final Commit

```bash
git add -A
git commit -m "Set up Antigravity agent"
```

---

## Completion

```
✅ Project scaffolded and set up!

Created:
- [framework] project with [language]
- 9 slash command workflows
- Planning skill + Create-skill
- AGENT.md with project context
- memory.md for session tracking

💡 Before building features, consider setting up your brand identity
   and core UI as your first /new-track. Tools like Google Stitch
   (stitch.withgoogle.com) can help generate UI mockups and ideas.

Next step:
→ /new-track to plan your first piece of work
```

## Error Handling

- **If scaffolding fails:** Show the error, suggest an alternative framework or manual setup
- **If the user isn't sure what to build:** Skip scaffolding, just set up a minimal structure (README.md, src/ folder), and proceed to Phase 2
- **If the user changes their mind mid-flow:** Start over from the relevant question — don't force them through the whole flow again
