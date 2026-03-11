# Workflow Categories & v10 Spec

## Problem

All workflows get installed into every project, even niche ones like Visual QA and Hormozi business frameworks. A v2 project updating to current version gets a change log that only describes the latest version's changes. The setup guide also doesn't mention the new living documents features (`/audit`, enhanced `/end-session`, enhanced `/status`, product.md/tech-stack.md templates).

## Solution

1. Split workflows into **Essential** (always installed) and **Additional** (offered during setup/update)
2. Version bump to v10 with updated install logic, change log, and completion message

## Categories

**Essential (11 workflows — always installed):**
- `new-track` — plan work
- `edit` — revise plans
- `implement` — execute with tracking
- `status` — project overview
- `update-memory` — log decisions
- `end-session` — session wrap-up
- `brainstorm` — full brainstorm
- `brainstorm-lite` — mid-work brainstorm
- `audit` — conductor document audit
- `create-skill` — create local skills
- `ux-design` — design direction

**Additional (5 workflows — offered as options):**
- `capture-target` — capture design data from a live site
- `recreate-site` — rebuild a site from captured data
- `compare-site` — fix build against captured target
- `offer-strategy` — Grand Slam Offer (Hormozi)
- `lead-strategy` — lead generation strategy

## Acceptance Criteria

- [ ] Essential workflows always installed without asking
- [ ] Additional workflows offered as a choice during setup and update
- [ ] Available Commands in templates reflects the essential/additional split
- [ ] AGENT_SETUP_GUIDE.md bumped to v10 with updated install logic
- [ ] new-project.md updated with same split
- [ ] Change log and completion message mention living documents features
- [ ] Update path from any old version (v1–v9) handles workflow categories correctly
