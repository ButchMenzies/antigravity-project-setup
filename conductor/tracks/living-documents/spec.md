# Living Documents Spec

## Problem

Conductor documents (`product.md`, `roadmap.md`, `tech-stack.md`) created during `/setup` become stale over time. `product.md` is never updated after creation and has no defined structure. `roadmap.md` only gets checkbox ticks during `/end-session` — no restructuring or reprioritization. `/status` doesn't read most conductor docs, so the agent misses project context. There's no mechanism to systematically reconcile docs against the actual codebase.

## Solution

Make conductor documents into living documents that stay accurate through:
1. A defined structure for `product.md` (Vision + Current State) and `tech-stack.md` — templated at creation, evolved over time
2. Updated setup workflows that create conductor docs with proper structure and guidance
3. Incremental updates at end-of-session (automatic for small changes, interactive for structural changes)
4. A richer status report that reads all conductor docs
5. A deep audit workflow to reconcile docs against the codebase on demand — including vision-to-actual migration

## Key Design Decision: Vision → Actual

`product.md` starts with a Vision section (from brainstorm/roadmap) and an empty Current State section. `/end-session` only adds to Current State as features are built. Vision items are never removed by end-session. During `/audit`, the agent reconciles: vision items already built get moved to Current State, irrelevant ones get proposed for removal with user confirmation. Over time, Current State grows and Vision shrinks naturally.

## Acceptance Criteria

- [ ] `product.md` has a defined template with Vision and Current State sections
- [ ] `tech-stack.md` has a defined template with clear structure
- [ ] `setup.md` and `new-project.md` create conductor docs using the new templates
- [ ] `/end-session` incrementally updates Current State in `product.md`, `tech-stack.md`, and `roadmap.md`
- [ ] Roadmap updates use a tiered approach: auto for checkboxes/detail, propose-and-confirm for structural changes
- [ ] `/status` reads and summarises `product.md`, `tech-stack.md`, and active `spec.md`
- [ ] `/audit` workflow exists and systematically compares conductor docs to codebase reality, including vision reconciliation
- [ ] No changes to `/critical-analysis` (stays Antigravity-specific)

## Out of Scope

- Changes to `/critical-analysis`
- Automated audit triggers (manual `/audit` is sufficient)
- Changes to how `roadmap.md` is initially structured (progressive detail with emojis stays as-is)
