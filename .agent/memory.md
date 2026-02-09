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

## Session Log

| Date | Summary |
|------|---------|
| 2026-02-09 | Implemented local-first skills architecture, created create-skill meta-skill, update document |
| 2026-02-10 | Built slash command system (8 workflows), rewrote bootstrapper, created USER_GUIDE.md, switched to GitHub skills, unified setup guides |
