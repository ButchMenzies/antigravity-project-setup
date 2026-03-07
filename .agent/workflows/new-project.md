---
description: Scaffold a brand new project — choose what to build, create folder structure, install dependencies
---

# New Project

Scaffold a new project from a blank folder. Handles scaffolding, Antigravity bootstrap, and project onboarding in one pass. **No need to run `/setup` afterward.**

> This workflow is triggered by the setup guide when it detects a completely empty project directory. At this point, `.agent/` does NOT exist.

---

## Phase 0: Brainstorm Gate

**Q0: Ready or brainstorm?** — "Do you already know what you're building, or would you like to brainstorm first?"

1. **I know what I'm building** → proceed to Phase 1
2. **I'd like to brainstorm first** → run `/brainstorm` inline

**Wait for user response before proceeding.**

If the user chooses to brainstorm:
- Run `/brainstorm` as-is — do not modify its rules or steer the conversation toward setup questions
- **Skip brainstorm's pre-flight** (steps 1–2) — `.agent/AGENT.md` and `.agent/memory.md` don't exist yet in an empty folder. Jump straight to the conversation
- When the brainstorm concludes naturally (user signals they're ready to proceed), **seamlessly transition** into Phase 1
- Use all context gathered during the brainstorm to **pre-fill and skip** Phase 1 questions where possible — the Q&A should feel lighter, not repetitive
- Do not announce the transition. The user should experience one continuous conversation, not two separate processes

---

## Phase 1: Plan & Scaffold

### Project Type (Q1)

What are you building?

1. Web app — interactive browser-based application (dashboards, SaaS, portals)
2. Website — static or content site (marketing, portfolio, blog, landing pages)
3. API / backend — server-side service or REST/GraphQL API
4. Mobile app — iOS, Android, or cross-platform
5. CLI tool — command-line utility
6. Library / package — reusable module
7. Workspace — strategy, planning, brainstorming, or content (no code)
8. Something else — describe it

**Wait for user response before proceeding.**

If user chose **Workspace (7)** → skip to [Phase 1W: Workspace Setup](#phase-1w-workspace-setup).

### Description (Q2)

"Describe what it should do in one or two sentences."

**Wait for user response.**

### Tech Stack (Q3)

Based on project type, **recommend a default stack** with a brief explanation. The user can accept or change it.

| Project Type | Default Recommendation | Alternatives to Offer |
|-------------|----------------------|----------------------|
| Web app | Next.js + TypeScript + Tailwind (App Router) | Vite + React + Tailwind; Vanilla HTML/CSS/JS |
| Website | Astro + Tailwind | Next.js; Vanilla HTML/CSS/JS |
| API / backend | Express + TypeScript | Fastify; FastAPI (Python) |
| Mobile app | React Native + Expo (TypeScript) | Flutter; Native Swift/Kotlin |
| CLI tool | Node.js + TypeScript | Go; Rust; Python |
| Library | TypeScript npm package | Python (PyPI); Go module |

**Wait for user response.**

### Work Style & Scope (Q4)

Ask both in one question:
- **Work style**: Plan first / Just go / Depends on size
- **Project scope**: Long-term (full project management) / Medium / Quick experiment

**Wait for user response.**

### Development Port (Q5)

What port? Default: 5010 (Antigravity default — avoids conflicts).

**Wait for user response.**

### Scaffold

Rules:
- **Use standard tooling** — `npx create-*`, `npm init`, `cargo init`, etc. Don't hand-roll when a CLI scaffolder exists
- **Non-interactive mode** — use `--yes`, `-y` flags
- **Check `--help` first** — before running any scaffolder
- **Install in current directory** — always `./`, not a subdirectory
- **TypeScript by default** unless user chose otherwise
- **Use npm** — always `--use-npm` when supported
- **Git init** if scaffolder doesn't create a repo

#### Framework Commands

**Next.js:**
```bash
npx -y create-next-app@latest ./ --help
npx -y create-next-app@latest ./ --ts --eslint --tailwind --app --src-dir --import-alias "@/*" --use-npm --yes
```

**Vite + React:**
```bash
npx -y create-vite@latest ./ --help
npx -y create-vite@latest ./ --template react-ts
npm install
npm install -D tailwindcss @tailwindcss/vite
```

**Astro:**
```bash
npx -y create-astro@latest ./ --help
npx -y create-astro@latest ./ --template minimal --typescript strict --install --no-git
npx astro add tailwind -y
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

Only create additional folders if the scaffolder didn't provide them. Don't create empty placeholder folders. Do **not** start the dev server during setup.

---

## Phase 1W: Workspace Setup

For non-code projects (strategy, planning, brainstorming, content).

### Description (Q2W)

"What's the purpose of this workspace? Describe in a sentence or two."

**Wait for user response.**

### Workspace Category (Q3W)

What kind of workspace?

1. Strategy / Business — marketing plans, offer design, consulting
2. Content / Writing — courses, blog, copywriting, email sequences
3. Research / Analysis — market research, data analysis, competitive analysis
4. General planning — brainstorming, project coordination

**Wait for user response.**

### Work Style & Scope (Q4W)

Same as Q4 above (plan first vs. just go, project scope).

**Wait for user response.**

### Create Structure

```bash
mkdir -p docs research output references
```

Create a `README.md` with the project description from Q2W.

Skip dev port question — not applicable.

---

## Phase 2: Bootstrap Antigravity

> **Run these as separate commands** — do not chain with `&&`.

**Step 1 — Clone:**
```bash
git clone --depth 1 https://github.com/ButchMenzies/antigravity-project-setup.git /tmp/ag-setup
```

**Step 2 — Create directories and copy files:**
```bash
mkdir -p .agent/workflows .agent/skills
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
cp /tmp/ag-setup/templates/skills-catalog.md .agent/skills-catalog.md
cp /tmp/ag-setup/templates/USER_GUIDE.md .agent/USER_GUIDE.md
```

**Step 3 — Clean up:**
```bash
rm -rf /tmp/ag-setup
```

**Do NOT install templates/AGENT.md or templates/memory.md** — Phase 3 creates these with real project data.

### Configure .gitignore

Append to existing `.gitignore` (don't replace existing content):
```
# Antigravity — keep workflows/skills tracked, ignore personal data
.agent/memory.md
.agent/memory-archive.md
.agent/pending-skill-uploads.md
.agent/current-plan.md
```

If `.gitignore` contains `.agent/` as a blanket ignore, remove that line — it breaks slash command autocomplete.

---

## Phase 3: Project Onboarding

We already know everything from Phase 1. Generate project files directly.

### AGENT.md

Use the appropriate template based on project type:
- **Code projects (Q1 options 1–6)** → Fetch `templates/AGENT-code.md` from `https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/templates/AGENT-code.md`
- **Workspace projects (Q1 option 7)** → Fetch `templates/AGENT-workspace.md` from `https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/templates/AGENT-workspace.md`

Populate all placeholder sections with real answers from the Q&A. Replace the title with the actual project name. For code projects, fill in Tech Stack, Local Development (port from Q5, start command from scaffold), Architecture, and Conventions. For workspace projects, fill in Goals, Audience, and Key Resources.

### memory.md

Use `templates/memory.md` as the base. Set work style from Q4/Q4W and add today's date to session log.

### Conductor (long-term projects only)

If scope = long-term (Q4/Q4W), create `conductor/` with project management artifacts:

```bash
mkdir -p conductor/tracks
```

Create these files:
- **`conductor/product.md`** — product context (from Q1/Q2 answers)
- **`conductor/tech-stack.md`** (code) or **`conductor/strategy.md`** (workspace) — technical or strategic decisions
- **`conductor/roadmap.md`** — phased project plan with progressive detail
- **`conductor/notes.md`** — user scratchpad for mid-session ideas

#### Roadmap Format

Populate from brainstorm output (Phase 0) if available, otherwise from Q&A context:

```markdown
# Project Roadmap

## Phase 1: [Name] 🔄
- [ ] [Concrete task or deliverable]
- [ ] [Concrete task or deliverable]
- [ ] [Concrete task or deliverable]

## Phase 2: [Name]
- [Outlined item]
- [Outlined item]

## Phase 3: [Name]
- [Sketched idea]
- [Sketched idea]
```

Use **progressive detail**: Phase 1 has concrete checkboxes, later phases get increasingly vague (just bullet points, no checkboxes). Mark Phase 1 with 🔄 (active).

#### Notes Format

```markdown
# Notes

> Drop ideas, questions, or reminders here. These will be reviewed during `/end-session`.
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
- [framework/workspace] project
- Slash command workflows
- 20+ skills (planning, debugging, database, components, deployment, and more)
- AGENT.md with project context
- memory.md for session tracking
[- conductor/ project management artifacts (if long-term)]

⚠️ Action Required: Close and reopen the project.
The IDE needs to re-scan to discover the new slash commands.

Next step:
→ Close and reopen the project
→ /new-track to plan your first piece of work
```

## Error Handling

- **If scaffolding fails:** Show error, suggest alternative framework or manual setup
- **If user isn't sure what to build:** Skip scaffolding, set up minimal structure (README, src/), proceed to Phase 2
- **If user changes mind mid-flow:** Restart from the relevant question, don't force the full flow
