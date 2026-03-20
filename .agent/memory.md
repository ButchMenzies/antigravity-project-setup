# Project Memory

> Agent-maintained record of lessons, preferences, and recent session context.

---

## Active Lessons

### Grep verification catches drift across files
After any terminology change, grep the entire repo for old terms before declaring done. During v11 update, grepping found 2 additional files (`new-project.md`, `critical-analysis.md`) and `AGENT_SETUP_GUIDE.md` still referencing deprecated v10 terms.

### Passive setup guides don't enforce behavior
Critical behaviors must be explicit rules, not suggestions buried in long documents. The old 713-line guide told the agent to update memory, but it was buried in templates and easily ignored. Fix: Core rules at the top of AGENT.md + mandatory checkpoints in `/implement`.

### Don't maintain two versions of the same thing
If content must appear in multiple places, have one source of truth. Local and portable setup guides drifted out of sync until unified into a single self-contained guide.

### Agent cannot access DevTools or computed CSS
Any workflow that requires browser data beyond DOM structure must explicitly state that the user provides the data. Agent tried to use browser tool for DevTools — it can't.

## User Preferences

- Work style: Plan first, then implement — reviews plans before coding
- Prefers interactive Q&A over template-filling
- Values single source of truth — avoids maintaining duplicate files
- Wants the system to work on any device, not just the local machine
- **IDE is always Antigravity** — all setup/restart instructions should reference Antigravity behavior (e.g., "close and reopen the project" not "start a new chat")
- **Don't shortcut or work around issues** — if a feature exists (e.g., MCP server in the IDE list), fix it properly instead of suggesting alternatives. Don't skip steps.
- **Skills catalogue is a database, not a distribution** — this project's `skills/` is a central store. Projects pull individual skills they need; don't push all skills to every project.

## Recent Sessions

### 2026-03-20
- v11: Two-tier document architecture redesign. Updated 18 files: 4 templates, 7 workflows (v2), setup.md scaffolding, new-project.md, update.md with Step 6b interactive migration, critical-analysis.md, AGENT_SETUP_GUIDE.md
- Removed tech-stack.md/strategy.md scaffolds. Added history.md, Project Principles, Development Workflow, Active Track + Backlog roadmap
- Critical analysis passed all 10 steps (1 minor: new-project.md 230 bytes over 12K)
- Migrated this repo's own memory.md to v11 structure

### 2026-03-12
- Created `/security-review` workflow — 8-phase platform-agnostic security review. Added to essential set (13 total). Critical analysis caught 4 integration issues, all fixed.
- Living conductor documents: product.md + tech-stack.md templates, enhanced /end-session with tiered roadmap + product/tech-stack updates, /status reads all conductor docs, created /audit workflow
- Workflow categories: 12 essential + 5 additional. Created update.md. Rewrote AGENT_SETUP_GUIDE.md as router. Bumped to v10
- Added version: 1 to all 22 workflow frontmatters. Added agent behaviour rules 11-13. Auto-update check in /end-session Step 9. Created /test workflow. VERSION file

### 2026-03-07
- Created skill bundles system: 14 shared skills + 7 bundle manifests. Updated /new-track pre-flight with structured skill matching
- Fixed both install paths to use concise loop. Removed 9 contaminated/redundant catalogue skills

### 2026-03-02
- Added Phase 0 Brainstorm Gate to /new-project — "ready or brainstorm?" before scaffolding. Critical analysis passed all 10 checks

### 2026-02-27
- End-session wrap-up. Completed /brainstorm workflow implementation and testing. Cleaned up temporary plans

---

## Archived History

> Key decisions and older session logs from v1–v10. Kept inline since this repo has no conductor/ directory.

<details>
<summary>Key Decisions (v1–v10)</summary>

| Date | Decision |
|------|----------|
| 2026-02-09 | Local-first skills architecture — skills are project-local (`.agent/skills/`), global library as inspiration |
| 2026-02-10 | Slash command system — rebuilt from 8 workflows with core rules in AGENT.md |
| 2026-02-10 | Unified setup guide — single self-contained `AGENT_SETUP_GUIDE.md` from GitHub |
| 2026-02-10 | GitHub for skills library — all references use raw GitHub URLs |
| 2026-02-10 | Three-way setup detection — fully set up / needs upgrade / fresh project |
| 2026-02-16 | Central skills catalogue with auto-sync — `skills/` is the central store |
| 2026-02-16 | Core Rule 6 — Honesty over confidence (don't guess about tools/platform) |
| 2026-02-16 | UX Design Foundation — `/ux-design` workflow + skill + templates |
| 2026-02-17 | Offer + Lead Strategy — Hormozi frameworks, smart folder detection, unified guide with version system |
| 2026-02-18 | v4 — Terminal discipline (Rule 7), browser URLs (Rule 8), port management (Rule 9, default 5010) |
| 2026-02-19 | v5 — Visual QA workflow system (capture-target → recreate-site / compare-site) |
| 2026-02-22 | Template extraction — AGENT-code.md + AGENT-workspace.md, eliminated core rules duplication |
| 2026-02-22 | Exclude setup-only workflows from user installs (setup, new-project, update-guide, critical-analysis) |
| 2026-02-22 | Critical analysis workflow for self-verification (10-step) |
| 2026-03-07 | Skill bundles per project type — 14 shared skills + 7 bundle manifests, install-all via loop |
| 2026-03-12 | Living conductor documents — product.md Vision/Current State model, tiered roadmap updates |
| 2026-03-12 | Workflow-level versioning — `version: N` in YAML frontmatter, version-aware updates |
| 2026-03-12 | Agent behaviour rules 11-13 (no fake options, do the work, read the code) |
| 2026-03-12 | Auto-update check in end-session — curls VERSION from GitHub |
| 2026-03-12 | /test workflow — 6-phase scope-adaptive testing (brainstorm → recon → design → execute → compile → act) |
| 2026-03-20 | v11 — Two-tier document architecture: Active Lessons + Recent Sessions, Active Track + Backlog, history.md archive, Project Principles in AGENT.md, tech-stack merged into product.md |

</details>

<details>
<summary>Session Log (v1–v10)</summary>

| Date | Summary |
|------|---------|
| 2026-02-09 | Implemented local-first skills architecture, created create-skill meta-skill |
| 2026-02-10 | Built slash command system (8 workflows), rewrote bootstrapper, USER_GUIDE.md |
| 2026-02-11 | Upgraded create-skill: depth tiers, fuzzy discovery, research phase |
| 2026-02-14 | Re-ran Antigravity agent setup for this project |
| 2026-02-15 | End-session wrap-up. Authenticated gh CLI |
| 2026-02-16 | Imported 13 skills, added Superpowers, Core Rule 6, UX Design Foundation |
| 2026-02-16 | Created /ux-design workflow + skill + 8 UX templates |
| 2026-02-17 | Created /offer-strategy and /lead-strategy. Unified setup guide. v3 |
| 2026-02-18 | v4: Terminal discipline, browser URLs, port management |
| 2026-02-19 | v5: Visual QA workflow system. Live tested on empoweredgrowth.co.nz |
| 2026-02-19 | Capture workflow v2: agent capability boundaries, platform detection |
| 2026-02-22 | Restructured onboarding: template extraction, workspace support, /critical-analysis |

</details>
