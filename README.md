# 🚀 Antigravity

An agent workflow system that gives AI coding assistants structured memory, reusable skills, and slash command workflows.

## What It Does

When bootstrapped into a project, Antigravity gives your AI agent:

- **Persistent memory** (`memory.md`) — decisions, lessons, and preferences survive across chat sessions
- **Slash command workflows** — `/new-track`, `/implement`, `/edit`, `/status`, `/update-memory`, `/end-session`, `/create-skill`, `/setup`
- **Local skills** — project-specific patterns the agent can reference and reuse
- **Structured planning** — phased implementation plans with progress tracking

## Quick Start

Paste the contents of [`AGENT_SETUP_GUIDE.md`](AGENT_SETUP_GUIDE.md) into a new project chat. The agent will:

1. Create the `.agent/` directory structure
2. Download all workflows and templates from this repo
3. Run `/setup` for interactive project onboarding

## What Gets Installed

```
your-project/
├── .agent/
│   ├── AGENT.md              ← Project context + core rules
│   ├── memory.md             ← Persistent memory
│   ├── skills-catalog.md     ← Skill index
│   ├── workflows/            ← 8 slash command definitions
│   │   ├── setup.md
│   │   ├── new-track.md
│   │   ├── edit.md
│   │   ├── implement.md
│   │   ├── status.md
│   │   ├── update-memory.md
│   │   ├── end-session.md
│   │   └── create-skill.md
│   └── skills/
│       └── create-skill/     ← Starter skill
│           └── SKILL.md
```

## Repo Structure

```
antigravity-project-setup/
├── AGENT_SETUP_GUIDE.md      ← Bootstrap prompt (paste into new chats)
├── USER_GUIDE.md             ← Human-readable reference
├── README.md                 ← This file
├── .agent/
│   ├── workflows/            ← Source of truth for all workflows
│   ├── skills/create-skill/  ← Source of truth for starter skill
│   └── skills-catalog.md
└── templates/
    ├── AGENT.md              ← Template for new projects
    ├── memory.md             ← Template for new projects
    └── skills-catalog.md     ← Template for new projects
```

## Slash Commands

| Command | What It Does |
|---------|-------------|
| `/setup` | Interactive project onboarding — scans codebase, configures AGENT.md |
| `/new-track` | Plan a new piece of work with spec and phased implementation plan |
| `/edit` | Revise an implementation plan before executing |
| `/implement` | Execute a plan with progress tracking and memory checkpoints |
| `/status` | Show project overview — tracks, recent memory, skills |
| `/update-memory` | Log a decision, lesson, preference, or issue |
| `/end-session` | Wrap up — update memory, note progress, prepare for next session |
| `/create-skill` | Create a reusable project-local skill from a repeating pattern |

## Related

- [Antigravity Awesome Skills](https://github.com/sickn33/antigravity-awesome-skills) — Global skills library (700+ skills for inspiration)

## License

MIT
