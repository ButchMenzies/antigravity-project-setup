---
version: 2
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

**Step 2 — Create directories and copy workflows based on project type:**

```bash
mkdir -p .agent/workflows .agent/skills
```

Install workflows based on project type from Q1. Use the mapping table below — ✅ means install, — means skip:

| Workflow | Code (Q1: 1-6) | Workspace (Q1: 7) |
|----------|:---:|:---:|
| `new-track.md` | ✅ | ✅ |
| `implement.md` | ✅ | ✅ |
| `edit.md` | ✅ | ✅ |
| `brainstorm.md` | ✅ | ✅ |
| `brainstorm-lite.md` | ✅ | ✅ |
| `status.md` | ✅ | ✅ |
| `update-memory.md` | ✅ | ✅ |
| `end-session.md` | ✅ | ✅ |
| `create-skill.md` | ✅ | ✅ |
| `audit.md` | ✅ | — |
| `test.md` | ✅ | — |
| `security-review.md` | ✅ | — |
| `ux-design.md` | ✅ | — |
| `review.md` | — | ✅ |
| `workspace-audit.md` | — | ✅ |
| `brand-design.md` | — | ✅ |

Copy each ✅ workflow from `/tmp/ag-setup/.agent/workflows/` to `.agent/workflows/`.

**Step 3 — Copy skills based on project type:**

| Skill | Code (Q1: 1-6) | Workspace (Q1: 7) |
|-------|:---:|:---:|
| `planning` | ✅ | ✅ |
| `copywriting` | ✅ | ✅ |
| `ux-design` | ✅ | ✅ |
| `voice-notes-triage` | ✅ | ✅ |
| `fix-bug` | ✅ | — |
| All other dev skills | ✅ | — |

For **code projects**: copy all skills from `/tmp/ag-setup/skills/*/` (current behaviour).
For **workspace projects**: copy only the skills marked ✅ above.

```bash
# Copy selected skills
for skill in planning copywriting ux-design voice-notes-triage; do  # workspace list — expand for code projects
  if [ -d "/tmp/ag-setup/skills/$skill" ]; then
    mkdir -p ".agent/skills/$skill"
    cp -r "/tmp/ag-setup/skills/$skill/"* ".agent/skills/$skill/"
  fi
done
```

```bash
cp /tmp/ag-setup/templates/skills-catalog.md .agent/skills-catalog.md
cp /tmp/ag-setup/templates/USER_GUIDE.md .agent/USER_GUIDE.md
```

**Step 4 — Offer additional workflows:**

For **code projects**:
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

For **workspace projects**:
```
📦 Additional workflows available:

- /offer-strategy — build a Grand Slam Offer (Hormozi framework)
- /lead-strategy — plan lead generation strategy (Hormozi framework)

Install any of these?
1. Install all
2. Skip — I don't need any of these
```

**Wait for user response.** Copy selected workflows from `/tmp/ag-setup/.agent/workflows/`.

**Step 5 — Claude Code support (optional):**

```
🔌 Claude Code Support

Are you also using the Claude Code extension or CLI?
If yes, we'll set up a .claude/ folder so Claude Code can access
all the same skills and workflows.

1. Yes — also set up for Claude Code
2. No — Antigravity only
```

**Wait for user response.**

If yes:
1. Create `.claude/skills/` directory
2. Mirror all skills from `.agent/skills/` → `.claude/skills/` (direct copy)
3. Convert workflows to Claude Code skills: for each `.agent/workflows/*.md` with `source: antigravity`, create `.claude/skills/<name>/SKILL.md` with `disable-model-invocation: true` added to frontmatter and `version:`/`source:` removed
4. Generate `CLAUDE.md` at project root from `.agent/AGENT.md` (with auto-generated header)

**Step 6 — Write version file:**
```bash
echo "12" > .agent/version
```

**Step 7 — Clean up:**
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

# Claude Code — generated from .agent/, regenerated on each update
.claude/
CLAUDE.md
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

Use `templates/memory.md` as the base. Set work style from Q4/Q4W and add today's date as the first Recent Sessions entry.

### Conductor (long-term projects only)

If scope = long-term (Q4/Q4W), create `conductor/` with project management artifacts:

```bash
mkdir -p conductor/tracks
```

Create these files:

#### `conductor/product.md`

Populate from Q&A/brainstorm. Structure: **Vision** (what we're building toward — written once, only modified by `/audit`), **Current State** (starts with "Project initialised. No features built yet." — updated by `/end-session`), **Features** (empty at start — built up as features ship), **Infrastructure Services** (for code projects: language, framework, styling, database, auth, hosting, port, start command — populated from Q3/Q5), **Security** (initialised as: "Last reviewed: Not yet. Run `/security-review` after your first deployment-ready build." — updated by `/security-review`).

#### `conductor/roadmap.md`

Populate from brainstorm output (Phase 0) if available, otherwise from Q&A context. Use the **Active Track + Backlog** format: Active Track has concrete checkboxes (`- [ ]`), Backlog has themed sections with outlined items, Cross-Cutting Notes captures items that span themes.

#### `conductor/history.md`

Create with empty scaffold: **Build Timeline** (table), **Archived Decisions**, **Archived Lessons Learned**, **Archived Session Log** (table), **Archived Design Documents**.

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
