# Changelog

All notable changes to the Antigravity agent system.

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
