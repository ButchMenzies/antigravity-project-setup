# Changelog

All notable changes to the Antigravity agent system.

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
