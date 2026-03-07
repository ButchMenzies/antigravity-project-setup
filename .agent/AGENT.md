# Antigravity Project Setup

> **Purpose**: Home base for the Antigravity agent setup system.
> This folder contains the bootstrapper, workflows, skills, and user guide.

## ⚠️ New Chat? Start Here
1. Read this file for context
2. Read `.agent/memory.md` for previous decisions
3. Check what's being worked on

## ⚠️ Core Rules (Always Apply)
1. **Before implementation**: Read the plan if one exists
2. **After completing a feature/fix**: Update `memory.md`
3. **Before ending a session**: Run `/end-session`
4. **When you notice repeating patterns**: Suggest creating a skill
5. **Don't guess about tools, settings, or platform behaviour.** If you're unsure, say so and verify first. Trust your coding knowledge; verify everything else.

## Available Commands
- `/setup` — interactive project onboarding
- `/new-project` — scaffold a new project
- `/new-track` — plan a new piece of work
- `/edit` — revise a plan
- `/implement` — execute with tracking
- `/status` — project status
- `/update-memory` — log decisions, lessons, preferences
- `/end-session` — wrap up session
- `/create-skill` — create a local skill
- `/ux-design` — define your product's design direction (personas, brand, visual identity)
- `/offer-strategy` — build a Grand Slam Offer
- `/lead-strategy` — plan lead generation strategy
- `/capture-target` — capture design data from a live site
- `/recreate-site` — rebuild a site from captured data
- `/compare-site` — fix an existing build against captured target data

---

## What This Project Is

This is the **setup project** — it contains the system that gets installed into other projects. It is NOT a coding project itself.

### Key Files

| File | Purpose |
|------|---------|
| `AGENT_SETUP_GUIDE.md` | Bootstrapper — paste into new projects |
| `USER_GUIDE.md` | Human reference — explains the system |
| `.agent/workflows/` | Slash command definitions (copied to projects) |
| `.agent/skills/create-skill/` | Skill creator (copied to projects) |
| `.agent/skills-catalog.md` | Skills reference |
| `updates/` | Patch documents for existing projects |

### Folder Structure

```
Antigravity Project Setup/
├── AGENT_SETUP_GUIDE.md        # Bootstrap prompt (paste into new chats)
├── USER_GUIDE.md               # Human-readable guide
├── README.md                   # GitHub repo readme
├── CHANGELOG.md                # Version history
├── .agent/
│   ├── AGENT.md                # This file
│   ├── memory.md               # Change history
│   ├── skills-catalog.md       # Skills reference
│   ├── skills/
│   │   ├── fix-bug/            # Structured debugging discipline
│   │   └── planning/           # Iterative planning principles
│   └── workflows/              # Slash commands (19 total — source of truth)
│       ├── brainstorm.md       # Full brainstorming session
│       ├── brainstorm-lite.md  # Mid-work brainstorm (stop and think)
│       ├── setup.md
│       ├── new-project.md
│       ├── new-track.md
│       ├── edit.md
│       ├── implement.md
│       ├── status.md
│       ├── update-memory.md
│       ├── end-session.md
│       ├── create-skill.md
│       ├── ux-design.md
│       ├── offer-strategy.md
│       ├── lead-strategy.md
│       ├── capture-target.md   # Visual QA: capture design data
│       ├── recreate-site.md    # Visual QA: build from captured data
│       └── compare-site.md     # Visual QA: fix build against target
├── templates/                  # Templates copied into new projects
│   ├── AGENT.md                # Starter AGENT.md (copy of AGENT-code.md)
│   ├── AGENT-code.md           # Core rules for code projects
│   ├── AGENT-workspace.md      # Core rules for workspace projects
│   ├── memory.md               # Starter memory.md
│   └── skills-catalog.md       # Starter skills catalog
├── skills/                     # Central skill pool + bundle manifests
│   ├── create-feature/         # Full vertical slice: DB → API → UI
│   ├── fix-bug/                # Structured debugging discipline
│   ├── database-change/        # Migration → RLS → types chain
│   ├── build-component/        # Reuse-first component creation
│   ├── build-page/             # Page construction with SEO
│   ├── create-endpoint/        # API route with validation + auth
│   ├── performance-audit/      # Bundle, rendering, caching checks
│   ├── auth-flow/              # Auth, protected routes, roles
│   ├── payments-stripe/        # Checkout, webhooks, subscriptions
│   ├── seo-audit/              # Meta tags, structure, technical SEO
│   ├── api-design/             # REST conventions, error formats
│   ├── deploy-vercel/          # Vercel deployment + env vars
│   ├── deploy-railway/         # Railway deployment + health checks
│   ├── package-publish/        # npm packaging + versioning
│   ├── planning/               # Planning principles (existing)
│   ├── ...                     # Other catalogue skills
│   └── bundles/                # Which skills to install per project type
│       ├── web-app.md          # 8 skills for SaaS/dashboards
│       ├── website.md          # 4 skills for marketing/content sites
│       ├── api-backend.md      # 6 skills for server-side services
│       ├── mobile-app.md       # 5 skills for React Native/Expo
│       ├── cli-tool.md         # 2 skills for CLI utilities
│       ├── library.md          # 3 skills for npm packages
│       └── workspace.md        # No dev skills (uses strategy skills)
└── updates/                    # Update documents for existing projects
```

**Conductor structure** (created in user projects with long-term scope):
```
conductor/
├── roadmap.md          # Phased project plan (progressive detail, status emojis)
├── product.md          # Product context
├── tech-stack.md       # Tech decisions (code) or strategy.md (workspace)
├── notes.md            # User scratchpad — processed during /end-session
└── tracks/
    └── [phase-name]/
        ├── spec.md     # Detailed spec from /new-track Q&A
        └── plan.md     # Implementation plan with tasks
```

## Maintenance Tasks

When modifying this project:
- Keep `AGENT_SETUP_GUIDE.md` as lightweight as possible — it's just a bootstrapper
- If workflows change, create an update document in `updates/`
- Test changes by bootstrapping a fresh project
- Update `USER_GUIDE.md` when adding/changing slash commands
