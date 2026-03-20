# Changelog

All notable changes to the Antigravity agent system.

---

## v11 — 2026-03-20

### Document Architecture Redesign

Two-tier active/archive system proven over 50+ sessions in production. Fixes scaling problems with unbounded memory growth, stale tech-stack files, and phase-based roadmaps.

### Added
- `conductor/history.md` — archive for completed phases, old decisions, old session logs
- `AGENT.md` → `## Project Principles` — active design decisions that influence every future feature
- `AGENT.md` → `## Development Workflow` — standard plan/build/test/commit/wrap-up cycle
- `update.md` Step 6b — interactive document structure migration for v10 → v11 upgrades

### Changed
- `memory.md` — `Key Decisions` and `Session Log` table replaced with `Active Lessons` + `Recent Sessions` (5-session rotation). Decisions route to Project Principles or history.md
- `roadmap.md` — numbered phases with 🔄/✅ emojis replaced with `Active Track` + themed `Backlog`
- `tech-stack.md` — merged into `product.md` as `## Infrastructure Services` (one less file)
- All 7 core workflows bumped to v2 — `end-session`, `implement`, `new-track`, `status`, `update-memory`, `audit`, `security-review`
- `setup.md` — conductor scaffolding uses new formats (Active Track roadmap, history.md, no tech-stack.md)
- `update.md` — interactive migration: auto-migrate or guided walkthrough for memory, conductor files, roadmap conversion

### Removed
- `tech-stack.md` scaffold from setup (merged into product.md)
- `strategy.md` scaffold from setup (merged into product.md)
- Step 4c in `end-session` (tech-stack update step)
- Step 1c in `status` (tech-stack details)
- Phase/emoji terminology throughout all workflows

---

## v9 — 2026-03-05

### Added
- **Roadmap & phase planning system** — `conductor/roadmap.md` provides a phased master plan with progressive detail (Phase 1 concrete, later phases sketched)
- **Notes scratchpad** — `conductor/notes.md` lets users drop ideas mid-session without derailing implementation; notes are processed during `/end-session`

### Changed
- `/new-track` — **rewritten** with roadmap situational awareness (3 states: active phase incomplete, next phase ready, no roadmap), brainstorm gate before planning, phase-scoped Q&A
- `/end-session` — new Step 1 processes `conductor/notes.md` (discuss, fold into roadmap, or discard); new Step 4 updates `conductor/roadmap.md` progress (✅/🔄 markers)
- `/brainstorm` — handoff protocol is now context-aware: updates `conductor/roadmap.md` if conductor exists, falls back to `.agent/current-plan.md` otherwise
- `/implement` — new Step 6a updates roadmap when a full phase completes
- `/status` — shows roadmap overview (phase names + completion status) and pending notes count
- `/new-project` Phase 3 — conductor section now creates `roadmap.md` + `notes.md` with format templates
- `/setup` Section 8 — conductor section now creates `roadmap.md` + `notes.md`
- Core Rule 1 in both AGENT templates — now references `conductor/roadmap.md` as primary plan location
- `AGENT_SETUP_GUIDE.md` — bumped to v9

---

## v5 — 2026-02-19

### Added
- `/capture-target` workflow — exhaustive design data capture from live sites (CSS values, typography, spacing, colours, layout)
- `/recreate-site` workflow — full pipeline to rebuild a site from captured data in any tech stack (Static HTML, React+Vite, Next.js)
- `/compare-site` workflow — compare and fix an existing build against captured target data
- `visual-qa` skill — shared engine for building Section Briefs, applying data to code, and gap analysis

### Details
- Capture workflow includes: two-layer extraction (container + elements), inner wrapper detection, hover state capture, font/meta capture, 5-screenshot-per-round limit, agent file renaming
- Both downstream workflows check for existing capture data before proceeding
- All three workflows are technology-agnostic — same captured data, different code output

---

## v4 — 2026-02-18

### Added
- Core Rule 7: **Terminal command discipline** — categorised timeouts for short/install/dev-server commands, max 2 status polls, no `&&` chaining, non-interactive mode enforcement
- Core Rule 8: **Browser & URLs** — always share the dev URL with the user after browser testing
- Core Rule 9: **Dev server port management** — port assigned during `/setup` or `/new-project`, enforced via `lsof` pre-check, default port 5010

### Changed
- `/setup` workflow: Updated port Q&A with Antigravity default (5010) and conflict note
- `/new-project` workflow: Added Development Port question (Q5) to Phase 1
- Both workflows: Updated generated AGENT.md templates with rules 6-9 and new Local Development section format (`Dev port`, `Dev URL`, `Start command`, `Key scripts`)
- `templates/AGENT.md`: Added rules 7-9, restructured Local Development section
- `AGENT_SETUP_GUIDE.md`: Bumped to v4, updated core rules merge block, updated completion message

---

## v3 — 2026-02-17

### Added
- `/offer-strategy` workflow — interactive Grand Slam Offer creation
- `/lead-strategy` workflow — interactive lead generation planning
- `offer-strategy` skill with 4 resource files (value equation, offer creation, enhancers, examples)
- `lead-strategy` skill with 4 resource files (core four, lead magnets, scaling, examples)

### Changed
- All workflows now use **smart folder detection** — scan for existing brand/strategy folders instead of hardcoding `.agent/` paths
- `/ux-design` workflow outputs to `brand/` (or detected folder) instead of `.agent/ux/`
- `/offer-strategy` outputs a single `offer.md` instead of multiple files
- `/lead-strategy` outputs a single `leads.md` instead of multiple files
- All workflows end with **actionable next steps**

---

## v2 — 2026-02-16

### Added
- `/ux-design` workflow — interactive design foundation (personas, brand, colors, typography)
- `ux-design` skill — reads/updates design artifacts during UI work
- Core Rule 6: "Don't guess about tools, settings, or platform behaviour"
- Superpowers library as skill discovery source in `/create-skill`

### Changed
- `/setup` now offers UX design foundation during onboarding (Section 5)
- `/new-track` checks for design files when planning UI work
- `/implement` prompts for UX knowledge updates after UI phases
- `planning` skill includes design-first principle for UI work
- `/end-session` scoped to current project only
- `/create-skill` syncs new skills to central catalogue

---

## v1 — 2026-02-10

### Added
- Slash command system (replaced template-based setup)
- 8 workflows: `/setup`, `/new-track`, `/edit`, `/implement`, `/status`, `/update-memory`, `/end-session`, `/create-skill`
- `planning` skill with iterative planning principles
- Core Rules 1-5 in AGENT.md
- `/new-project` workflow for blank project scaffolding
