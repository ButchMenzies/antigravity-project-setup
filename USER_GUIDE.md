# Antigravity User Guide

> **For the human.** This explains how the Antigravity agent setup system works, what the slash commands do, and how to use them.

---

## What Is This?

Antigravity is a lightweight project management system for AI coding agents. It gives your agent:

- **Memory** across chat sessions (decisions, lessons, preferences)
- **Structured workflows** for planning and building features
- **Skills** — reusable project-specific knowledge
- **Slash commands** to control the agent's workflow

## Getting Started

### First Time Setup

1. Open a new project in your IDE
2. Paste the contents of `AGENT_SETUP_GUIDE.md` into the chat
3. The agent bootstraps the system (creates folders, copies workflow files)
4. Type `/setup` and answer the agent's questions about your project

After `/setup`, your project will have context files, memory, and skill suggestions ready to go.

### Returning to a Project

Start a new chat. The agent will:
1. Read `.agent/AGENT.md` for project context
2. Read `.agent/memory.md` for previous decisions
3. Be ready to work, with full context from past sessions

---

## Slash Commands

Type `/` in the chat to see all available commands. Here's what each one does:

### `/setup` — Project Onboarding
**When**: First time setting up a project  
**What it does**: Asks you questions about the project, scans the codebase, discovers relevant skills, and populates all project files.

### `/new-track` — Plan New Work
**When**: Starting a new feature, bug fix, or refactor  
**What it does**: Guides you through defining what to build, then creates a phased plan with tasks. Always do this before the agent starts coding — it prevents the agent from going off in the wrong direction.

### `/edit` — Revise a Plan  
**When**: A plan needs tweaking before you implement  
**What it does**: Shows the current plan and walks through changes. Use this instead of saying "change step 3" — it keeps the plan document consistent.

### `/implement` — Build with Tracking
**When**: Ready to execute a plan  
**What it does**: Works through the plan task by task, tracking progress. Includes **automatic memory checkpoints** after each phase and prompts to create skills when it notices repeating patterns.

### `/status` — Project Overview
**When**: Checking where things stand  
**What it does**: Shows active tracks, recent memory entries, available skills, and suggested next actions.

### `/update-memory` — Log Something Important
**When**: The agent made a decision worth remembering, or you solved a tricky problem  
**What it does**: Adds a structured entry to `memory.md` so future sessions have the context.

### `/end-session` — Wrap Up
**When**: Before ending a chat session  
**What it does**: Updates memory, saves track progress, notes unfinished work for next time. **This is how context transfers between sessions.**

### `/create-skill` — Capture a Pattern
**When**: You notice the agent doing the same multi-step process repeatedly  
**What it does**: Creates a reusable skill file that the agent can reference in future sessions.

---

## The Typical Workflow

```
1. Start project:     Paste setup guide → /setup
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
│   ├── skills/               ← Project-specific skills
│   │   └── planning/         ← Planning skill
│   └── workflows/            ← Slash command definitions
│       ├── setup.md
│       ├── new-track.md
│       ├── edit.md
│       ├── implement.md
│       ├── status.md
│       ├── update-memory.md
│       ├── end-session.md
│       └── create-skill.md
└── conductor/                ← Only if you chose long-term project during /setup
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
- Suggested during `/setup` (based on your tech stack)
- Prompted during `/implement` (when patterns repeat)
- Created on demand via `/create-skill`

### Tracks (Optional)
For long-term projects, tracks provide structured project management:
- Each piece of work gets a **track** with a spec and plan
- Plans break work into **phases** with **tasks**
- Progress is tracked at the task level
- Tracks live in `conductor/tracks/`

### Core Rules
Even if you don't use slash commands, the agent follows these rules (from AGENT.md):
1. Read the plan before implementing (if one exists)
2. Update memory after completing work
3. Run end-session checklist before finishing
4. Suggest skills when patterns repeat

---

## Tips

- **Always run `/new-track` before asking the agent to build something big.** This is the biggest improvement you'll notice — the agent works much better with a plan.
- **Run `/end-session` before closing a chat.** This is how the agent remembers what happened.
- **Don't worry about `/update-memory` during work** — `/implement` handles it automatically at phase boundaries.
- **Use `/edit` instead of "change step 3"** — it keeps the plan document consistent.
- **Skills compound over time.** The more skills you create, the better the agent gets at your specific project.

---

## Troubleshooting

**Agent didn't follow the workflow:** Check that `.agent/AGENT.md` has the Core Rules section. The agent reads this at the start of every chat.

**Agent forgot previous context:** Check `.agent/memory.md` — was `/end-session` run last time? If memory is empty, context was lost.

**Slash commands not showing:** Check that `.agent/workflows/` has the `.md` files. Re-run the bootstrapper if needed. Also check that `.gitignore` does NOT contain `.agent/` — blanket-ignoring it prevents Antigravity from discovering workflows. Use selective ignores instead (see setup guide).

**Agent went off-plan:** Next time, use `/new-track` to create a plan first, then `/implement` to execute it with tracking.

**Slash commands not showing:** If workflows don't appear in the `/` menu after setup, try restarting the chat session to force a re-scan.
