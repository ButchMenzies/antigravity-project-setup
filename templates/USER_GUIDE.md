# Antigravity User Guide

> **For the human.** This explains how the Antigravity agent setup system works, what the slash commands do, and how to use them.

---

## What Is This?

Antigravity is a lightweight project management system for AI coding agents. It gives your agent:

- **Memory** across chat sessions (decisions, lessons, preferences)
- **Structured workflows** for planning and building features
- **Skills** — reusable project-specific knowledge
- **Slash commands** to control the agent's workflow
- **Terminal discipline** — prevents the agent from freezing on stuck commands
- **Port management** — consistent dev server ports per project

## Getting Started

### First Time Setup

1. Open a new project in your IDE
2. Paste the contents of `AGENT_SETUP_GUIDE.md` into the chat
3. The agent bootstraps the system (creates folders, copies workflow files)
4. Close and reopen the project, then follow the onboarding instructions

After onboarding, your project will have context files, memory, and skill suggestions ready to go.

### Returning to a Project

Start a new chat. The agent will:
1. Read `.agent/AGENT.md` for project context
2. Read `.agent/memory.md` for previous decisions
3. Be ready to work, with full context from past sessions

---

## Slash Commands

Type `/` in the chat to see all available commands. Here's what each one does:

### Core Workflow

| Command | When to Use | What It Does |
|---------|-------------|--------------|
| `/new-track` | Starting new work | Guides you through defining what to build, creates a phased plan |
| `/edit` | Plan needs tweaking | Shows the current plan and walks through changes |
| `/implement` | Ready to build | Executes the plan task by task with memory checkpoints |
| `/status` | Checking progress | Shows active tracks, recent memory, available skills |
| `/update-memory` | Something worth remembering | Adds structured entry to memory.md |
| `/end-session` | Before closing chat | Saves progress, updates memory, notes what to resume |
| `/create-skill` | Repeating pattern spotted | Creates a reusable skill for future sessions |

### Project Setup

| Command | When to Use | What It Does |
|---------|-------------|--------------|
| `/new-project` | Empty folder, starting fresh | Scaffolds framework, creates structure, bootstraps Antigravity |
| `/setup` | Existing project, first onboarding | Interactive Q&A to populate project context |

### Strategy & Design

| Command | When to Use | What It Does |
|---------|-------------|--------------|
| `/ux-design` | Before building UI | Interactive design foundation — personas, brand, colors, typography |
| `/offer-strategy` | Defining your product offer | Grand Slam Offer creation — value stack, bonuses, guarantee, pricing |
| `/lead-strategy` | Planning lead generation | Lead channels, lead magnets, outreach, and scaling plan |

---

## The Typical Workflow

```
1. Start project:     Paste setup guide → onboarding
2. Plan work:         /new-track
3. Refine (optional): /edit
4. Build:             /implement
5. Wrap up:           /end-session
6. Next feature:      /new-track → repeat
```

## File Structure

After setup, your project will have:

```
project/
├── .agent/
│   ├── AGENT.md              ← Project context (agent reads this first)
│   ├── memory.md             ← Decisions, lessons, preferences
│   ├── skills-catalog.md     ← Index of available skills
│   ├── version               ← Current Antigravity version
│   ├── skills/               ← Project-specific skills
│   │   ├── planning/         ← Planning principles
│   │   ├── ux-design/        ← UX design foundation
│   │   ├── offer-strategy/   ← Offer creation framework
│   │   ├── lead-strategy/    ← Lead generation framework
│   │   └── voice-notes-triage/ ← Voice note processing
│   └── workflows/            ← Slash command definitions
│       ├── new-track.md
│       ├── edit.md
│       ├── implement.md
│       ├── status.md
│       ├── update-memory.md
│       ├── end-session.md
│       ├── create-skill.md
│       ├── new-project.md
│       ├── ux-design.md
│       ├── offer-strategy.md
│       └── lead-strategy.md
└── conductor/                ← Only if using long-term project tracking
    ├── tracks.md
    ├── product.md
    ├── tech-stack.md
    └── workflow.md
```

## Key Concepts

### Memory
The agent maintains `.agent/memory.md` with decisions, lessons, and preferences. This is how context survives between chat sessions. The agent updates it:
- Automatically during `/implement` (after each phase)
- Automatically during `/end-session`
- On demand via `/update-memory`

### Skills
Skills are project-specific knowledge files that teach the agent how to do things your way. They live in `.agent/skills/` and are:
- Suggested during onboarding (based on your tech stack)
- Prompted during `/implement` (when patterns repeat)
- Created on demand via `/create-skill`

### Tracks (Optional)
For long-term projects, tracks provide structured project management:
- Each piece of work gets a **track** with a spec and plan
- Plans break work into **phases** with **tasks**
- Progress is tracked at the task level
- Tracks live in `conductor/tracks/`

### Core Rules
The agent always follows these rules (from AGENT.md):
1. Read the plan before implementing (if one exists)
2. Scan skills before starting any task
3. Update memory after completing work
4. Run end-session checklist before finishing
5. Suggest skills when patterns repeat
6. Don't guess about tools or platform behaviour — verify first
7. **Terminal discipline** — categorised timeouts, max 2 polls, no command chaining
8. **Browser URLs** — always share the dev URL after testing
9. **Port management** — use the assigned port, check before starting, clean up orphans

---

## Tips

- **Always run `/new-track` before asking the agent to build something big.** This is the biggest improvement you'll notice — the agent works much better with a plan.
- **Run `/end-session` before closing a chat.** This is how the agent remembers what happened.
- **Don't worry about `/update-memory` during work** — `/implement` handles it automatically at phase boundaries.
- **Use `/edit` instead of "change step 3"** — it keeps the plan document consistent.
- **Skills compound over time.** The more skills you create, the better the agent gets at your specific project.
- **Port conflicts?** The agent checks before starting dev servers. If a port is stuck from a previous session, it'll offer to clean it up.

---

## Troubleshooting

**Agent didn't follow the workflow:** Check that `.agent/AGENT.md` has the Core Rules section. The agent reads this at the start of every chat.

**Agent forgot previous context:** Check `.agent/memory.md` — was `/end-session` run last time? If memory is empty, context was lost.

**Slash commands not showing:** Check that `.agent/workflows/` has the `.md` files. Re-run the bootstrapper if needed. Also check that `.gitignore` does NOT contain `.agent/` — blanket-ignoring it prevents Antigravity from discovering workflows. Use selective ignores instead (see setup guide).

**Agent went off-plan:** Next time, use `/new-track` to create a plan first, then `/implement` to execute it with tracking.

**Slash commands not showing after setup:** Close and reopen the project to force the IDE to re-scan the workspace.

**Agent freezing on commands:** This should be fixed by Core Rule 7 (terminal discipline). If it still happens, check that AGENT.md has the v4 core rules section.

**Port conflict:** The agent uses `lsof` to check the port before starting. If you see a conflict, it's likely an orphaned dev server from a previous session — let the agent kill it, or run `kill <PID>` manually.
