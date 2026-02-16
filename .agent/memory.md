# Project Memory

> Agent-maintained record of decisions, lessons, and context.
> Update after completing features, making decisions, or solving problems.

---

## Key Decisions

### 2026-02-09 Local-first skills architecture
**Context**: Skills were only referenced from a global library, making them hard to discover and use.
**Decision**: Skills are now project-local first (`.agent/skills/`), with the global library as inspiration.
**Rationale**: Local skills are tailored to the project, always available, and easier to maintain.

### 2026-02-10 Slash command system
**Context**: The old setup guide was 713 lines of templates that the agent filled in passively. Agent wasn't updating memory or creating skills proactively.
**Decision**: Rebuilt the entire system as 8 slash command workflows with core rules in AGENT.md.
**Rationale**: Slash commands are interactive (Q&A), enforce memory checkpoints, and prompt for skill creation. Core rules ensure critical behaviors happen even without explicit commands.

### 2026-02-10 Unified setup guide (single file)
**Context**: Had both a local guide (uses `cp`) and a portable guide (embedded content). Two files to maintain.
**Decision**: Merged into one self-contained `AGENT_SETUP_GUIDE.md` with all workflow content embedded inline.
**Rationale**: One file to maintain, works on any device, no local path dependencies.

### 2026-02-10 GitHub for skills library
**Context**: Skills library was referenced via local path (`~/Desktop/Antigravity/antigravity-awesome-skills-main/`). Static, outdated.
**Decision**: All references now use `https://github.com/sickn33/antigravity-awesome-skills` and raw GitHub URLs.
**Rationale**: Always up to date (713+ skills), works on any device, no local files needed.

### 2026-02-10 Three-way setup detection
**Context**: Bootstrapper would stop if AGENT.md existed, even if workflows weren't installed. Broke upgrade path.
**Decision**: Detection now checks for workflows specifically: fully set up / needs upgrade / fresh project.
**Rationale**: Handles the case where a project used the old template-based setup and needs workflow installation.

### 2026-02-16 Central skills catalogue with auto-sync
**Context**: Skills created in individual projects were never consolidated. 14 unique skills across 5 projects.
**Decision**: `skills/` in this project is the central catalogue. `create-skill.md` Step 8a auto-syncs new skills here. obras/superpowers added as a third discovery source.
**Rationale**: One place to find all reusable skills. iCloud Desktop syncs path across devices.

### 2026-02-16 Core Rule 6 — Honesty over confidence
**Context**: Agent was guessing about IDE features and platform behaviour instead of checking.
**Decision**: Added Core Rule 6: don't guess about tools/settings/platform. Trust coding knowledge, verify everything else.
**Rationale**: AI training data goes stale fast. Tool/platform questions need verification; coding patterns don't.

### 2026-02-16 UX Design Foundation — hybrid workflow + skill
**Context**: Products built by AI agents look generic ("vibe-coded") because there's no design strategy driving decisions. Existing skills cover implementation patterns but not the "why".
**Decision**: Created `/ux-design` workflow (interactive discovery → creates `.agent/ux/` knowledge base) + `ux-design` skill (two-way — reads artifacts before building UI, prompts updates after). Integrated into `/setup`, `/new-track`, `/implement`, and `planning`.
**Rationale**: The workflow captures the vision once, artifacts persist it, and the skill enforces it every time UI is built. Living documents that grow with the product.

## Lessons Learned

### 2026-02-10 Passive setup guides don't enforce behavior
**Problem**: The old 713-line guide told the agent to update memory, but it was buried in templates and easily ignored.
**Fix**: Core rules at the top of AGENT.md + mandatory checkpoints in `/implement` workflow.
**Prevention**: Critical behaviors must be explicit rules, not suggestions buried in long documents.

### 2026-02-10 Don't maintain two versions of the same thing
**Problem**: Local and portable setup guides would drift out of sync.
**Fix**: Unified into a single self-contained guide.
**Prevention**: If content must appear in multiple places, have one source of truth.

## User Preferences

- Work style: Plan first, then implement — reviews plans before coding
- Prefers interactive Q&A over template-filling
- Values single source of truth — avoids maintaining duplicate files
- Wants the system to work on any device, not just the local machine
- **IDE is always Antigravity** — all setup/restart instructions should reference Antigravity behavior (e.g., "close and reopen the project" not "start a new chat")
- **Don't shortcut or work around issues** — if a feature exists (e.g., MCP server in the IDE list), fix it properly instead of suggesting alternatives. Don't skip steps.
- **Skills catalogue is a database, not a distribution** — this project's `skills/` is a central store. Projects pull individual skills they need; don't push all skills to every project.

## Session Log

| Date | Summary |
|------|---------|
| 2026-02-09 | Implemented local-first skills architecture, created create-skill meta-skill, update document |
| 2026-02-10 | Built slash command system (8 workflows), rewrote bootstrapper, created USER_GUIDE.md, switched to GitHub skills, unified setup guides |
| 2026-02-11 | Upgraded create-skill: merged skill into workflow, added depth tiers (Quick/Solid/Deep), fuzzy discovery, research phase, resources folder. Added project restart instructions for Antigravity IDE. |
| 2026-02-14 | Re-ran Antigravity agent setup for this project. |
| 2026-02-15 | End-session wrap-up. Authenticated `gh` CLI as ButchMenzies. Logged preference: don't shortcut issues. |
| 2026-02-16 | Imported 13 skills from 5 projects into central `skills/` catalogue. Added Superpowers as third discovery source. Added auto-sync to create-skill workflow (Step 8a). Added Core Rule 6 (honesty over confidence) to templates/AGENT.md, AGENT_SETUP_GUIDE.md, and .agent/AGENT.md. Created update patch for existing projects. Fixed end-session scoping to prevent cross-project memory contamination. |
| 2026-02-16 | Created UX Design Foundation: `/ux-design` workflow + `ux-design` skill + 8 UX templates. Integrated into `/setup` (Section 5), `/new-track` (pre-flight), `/implement` (UX checkpoint), `planning` (design-first). Updated bootstrapper and created update patch for existing projects. |
