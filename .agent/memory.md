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

### 2026-02-17 Offer + Lead Strategy skills and unified setup guide
**Context**: Needed Hormozi frameworks baked into the agent system. Also had a fragile update process — separate update files with local paths.
**Decision**: Created `/offer-strategy` and `/lead-strategy` workflows + skills. All three workflows (UX, offer, leads) now use smart folder detection. Unified the setup guide with a version system (`.agent/version`) so one file handles both fresh installs and updates, all from GitHub.
**Rationale**: Smart folders put output where humans look (not buried in `.agent/`). Unified guide eliminates update patches and works for external users too.

### 2026-02-18 v4 — Terminal discipline, browser URLs, port management
**Context**: Terminal commands frequently freeze the agent in polling loops. Browser tool opens separate Chrome profiles. Dev server ports are random and conflicting.
**Decision**: Added Core Rules 7 (categorised terminal timeouts), 8 (always share dev URL with user), 9 (port assigned during /setup, enforced via lsof check). Port question added to both `/setup` and `/new-project` workflows. Default port: 5010.
**Rationale**: Categorised timeouts are better than a blanket rule — dev servers and install commands need different handling. Port-in-AGENT.md is simple and always-visible. 5010 avoids conflicts with common services (3000, 5173, 5432, 8080).

### 2026-02-19 v5 — Visual QA workflow system
**Context**: Rebuilding websites from reference requires capturing exhaustive CSS data, then generating code from that data. Live test proved the approach works (7 sections rebuilt from data alone) but revealed gaps in capture (missing inner wrappers, nested backgrounds, hover states, font sources).
**Decision**: Created three-workflow pipeline: `/capture-target` (standalone input) → `/recreate-site` (build from scratch) or `/compare-site` (fix existing build). Plus `visual-qa` skill as the shared engine. Renamed `/design-audit` to `/compare-site` for clarity.
**Rationale**: Technology-agnostic capture means same data works for any stack. Both downstream workflows check for capture data before proceeding, guiding users to `/capture-target` first.

### 2026-02-22 Template extraction — eliminate duplication
**Context**: AGENT.md core rules (~2,600 chars) were duplicated in 4 files. Both `setup.md` (13,553) and `new-project.md` (16,182) were over the 12K workflow limit. No path for non-code projects.
**Decision**: Extracted core rules into `templates/AGENT-code.md` and `templates/AGENT-workspace.md`. Workflows now reference templates instead of inlining. `templates/AGENT.md` kept as a copy of AGENT-code.md for backwards compatibility.
**Rationale**: Single source of truth for rules. Halved both workflow sizes. Workspace template enables strategy/content/research projects.

### 2026-02-22 Exclude setup-only workflows from user installs
**Context**: `setup.md`, `new-project.md`, `update-guide.md` were being installed into every user project even though they're only used during initial setup or for maintaining this repo.
**Decision**: These plus `critical-analysis.md` are now excluded via `rm -f` after the wildcard copy. 17 total → 13 installed.
**Rationale**: Users don't need meta-workflows cluttering their slash command list.

### 2026-02-22 Critical analysis workflow for self-verification
**Context**: After making changes to the setup system, multiple consistency issues were discovered only through manual analysis (command lists out of sync, skill installs missing, stale paths).
**Decision**: Created `/critical-analysis` — a 10-step verification workflow that checks character counts, cross-references, template sync, hardcoded paths, scenario traces, version consistency, and GitHub URLs.
**Rationale**: Codifies all known failure modes. Ensures nothing gets missed regardless of session context.

## Lessons Learned

### 2026-02-10 Passive setup guides don't enforce behavior
**Problem**: The old 713-line guide told the agent to update memory, but it was buried in templates and easily ignored.
**Fix**: Core rules at the top of AGENT.md + mandatory checkpoints in `/implement` workflow.
**Prevention**: Critical behaviors must be explicit rules, not suggestions buried in long documents.

### 2026-02-10 Don't maintain two versions of the same thing
**Problem**: Local and portable setup guides would drift out of sync.
**Fix**: Unified into a single self-contained guide.
**Prevention**: If content must appear in multiple places, have one source of truth.

### 2026-02-19 Agent cannot access DevTools or computed CSS
**Problem**: Agent tried to capture design data itself using the browser tool — opening DevTools, estimating heights, taking screenshots of CSS panels. It can't do any of this.
**Fix**: Added explicit agent capability boundary rule to `capture-target.md`. Agent orchestrates capture (tells user what to inspect), user does all DevTools work.
**Prevention**: Any workflow that requires browser data beyond DOM structure must explicitly state that the user provides the data.

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
| 2026-02-17 | Created `/offer-strategy` and `/lead-strategy` skills + workflows (Hormozi frameworks). Updated all three workflows (UX, offer, leads) with smart folder detection and single-file output. Unified `AGENT_SETUP_GUIDE.md` with version system — one guide handles fresh installs and updates from GitHub. Created `CHANGELOG.md`. Added `voice-notes-triage` to standard install. Committed and pushed v3. |
| 2026-02-18 | v4: Added Core Rules 7 (terminal command discipline), 8 (browser URL sharing), 9 (dev server port management). Updated templates/AGENT.md, AGENT_SETUP_GUIDE.md, setup.md, and new-project.md. Port question added to both /setup and /new-project workflows. Default port 5010. |
| 2026-02-19 | v5: Created Visual QA workflow system. Built and live-tested capture-target, recreate-site and compare-site workflows + visual-qa skill. Live test rebuilt empoweredgrowth.co.nz from captured data (7 sections, all verified). Critical analysis found and fixed 3 P0 bugs + 4 P1 gaps. Renamed design-audit to compare-site. Updated AGENT.md (16 commands), CHANGELOG, skills-catalog, AGENT_SETUP_GUIDE.md (v5 bootstrapper). Created BOOTSTRAP.md — tiny 4-line snippet for Wispr Flow instead of pasting the full guide. Committed and pushed. |
| 2026-02-19 | Capture workflow v2: Live test on Squarespace site revealed agent trying to capture data itself and poor spacing enforcement. Added agent capability boundary rule, platform detection (Step 0), template-builder nesting guidance, positioning properties, upload limit clarification, Computed tab emphasis. Added build enforcement to recreate-site (min-height rule, Tailwind bracket warning, vertical sanity check). Tested agent DOM/JS capabilities — confirmed no DevTools access. Committed and pushed. |
| 2026-02-22 | Restructured onboarding: extracted AGENT.md core rules into `AGENT-code.md` and `AGENT-workspace.md` templates. Rewrote setup.md (13,553→6,248), new-project.md (16,182→8,682), create-skill.md (12,138→11,196). Added workspace/non-code project support. Synced Available Commands across all templates, matched skill installs between both paths. Excluded 4 setup-only workflows from user installs (13 installed). Created `/critical-analysis` workflow (10-step verification). Fixed hardcoded paths. Committed and pushed. |
| 2026-02-27 | End-session wrap-up. Completed implementation of the `/brainstorm` workflow and testing. Cleaned up temporary plans. |
