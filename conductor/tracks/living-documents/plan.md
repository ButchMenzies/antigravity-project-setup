# Implementation Plan: Living Conductor Documents

## Spec Summary

Conductor documents go stale because they're never updated after creation. This track defines proper templates for `product.md` and `tech-stack.md` (with a Vision/Current State model), updates setup workflows to use them, enhances `/end-session` with tiered document updates, enriches `/status` to read all conductor docs, and creates a new `/audit` workflow for deep codebase-to-doc reconciliation.

## Acceptance Criteria

- [x] `product.md` and `tech-stack.md` have defined templates
- [x] `setup.md` and `new-project.md` create conductor docs with proper structure
- [x] `/end-session` incrementally updates `product.md`, `tech-stack.md`, and `roadmap.md`
- [x] Roadmap updates use tiered approach (auto for small, propose for structural)
- [x] `/status` reads all conductor docs
- [x] `/audit` workflow exists with vision reconciliation
- [x] No changes to `/critical-analysis`

---

## Phase 1: Define Templates & Update Setup

### Tasks

- [ ] Task 1.1: Define `product.md` template structure
  - Sections: Product Name, Vision (from brainstorm/roadmap — what we're building toward), Current State (starts near-empty, grows via end-session updates), Features (detailed list of what exists now)
  - Include inline guidance comments so the agent knows how to populate each section

- [ ] Task 1.2: Define `tech-stack.md` template structure
  - Sections: Core Stack (language, framework, runtime), Infrastructure (database, hosting, auth), Key Dependencies (with versions), Development (dev server, port, tooling)
  - Should capture decisions and rationale, not just a list

- [ ] Task 1.3: Update `setup.md` Section 8
  - Replace the vague "product context" instruction with the `product.md` template
  - Replace the vague "technical decisions" instruction with the `tech-stack.md` template
  - Populate from Q&A answers and codebase scan

- [ ] Task 1.4: Update `new-project.md` conductor section
  - Same templates as setup.md
  - Populate from scaffold choices and brainstorm output

### Verification

- [ ] Read updated `setup.md` and verify templates are embedded in Section 8
- [ ] Read updated `new-project.md` and verify conductor creation matches
- [ ] Both files still under 12K characters

---

## Phase 2: Enhance `/end-session`

### Tasks

- [ ] Task 2.1: Add Step 4b — "Update Product Context"
  - Read `conductor/product.md`
  - Review what was built/changed this session
  - Add new features to Current State, update changed features
  - Never remove Vision items (that's `/audit`'s job)
  - Only runs if `conductor/product.md` exists

- [ ] Task 2.2: Add Step 4c — "Update Tech Stack"
  - Read `conductor/tech-stack.md`
  - Check if new dependencies, services, or tools were added this session
  - Update accordingly
  - Only runs if `conductor/tech-stack.md` exists

- [ ] Task 2.3: Enhance Step 4 — "Tiered Roadmap Updates"
  - **Tier 1 (auto):** Tick completed items, move 🔄 emoji, flesh out vague items in next phase
  - **Tier 2 (propose & confirm):** Phase reordering, new phases, removing phases, changing next phase direction
  - Clear inline guidance on what triggers Tier 2

### Verification

- [ ] Read updated `end-session.md` and verify all three sub-steps
- [ ] Verify Tier 1/Tier 2 distinction is clear
- [ ] Verify all steps conditional on conductor files existing
- [ ] File still under 12K characters

---

## Phase 3: Enhance `/status`

### Tasks

- [ ] Task 3.1: Add "Product Context" section
  - After Step 1 (Project Context), read and summarise `conductor/product.md`
  - Show current state summary and feature count

- [ ] Task 3.2: Add "Tech Stack" section
  - Read and summarise `conductor/tech-stack.md`

- [ ] Task 3.3: Show active spec with tracks
  - When showing active tracks (Step 3), also read the track's `spec.md`
  - Show acceptance criteria progress

### Verification

- [ ] Read updated `status.md` and verify new sections
- [ ] All sections conditional on files existing
- [ ] Output structure reads cleanly

---

## Phase 4: Create `/audit` workflow

### Tasks

- [ ] Task 4.1: Create `.agent/workflows/audit.md`
  - Step 1: Read all conductor documents
  - Step 2: Scan actual codebase (file structure, routes, DB schema, modules, dependencies, config)
  - Step 3: Compare and flag discrepancies
  - Step 4: Vision reconciliation — items built → propose moving to Current State; items no longer relevant → propose removal with user confirmation
  - Step 5: Present findings as structured report
  - Step 6: Apply approved changes
  - Technology-agnostic (works for any project type — code, workspace, hybrid)

- [ ] Task 4.2: Add `/audit` to Available Commands
  - Update `templates/AGENT-code.md`
  - Update `templates/AGENT-workspace.md`
  - Update `templates/AGENT.md` (sync with AGENT-code.md)
  - Update `.agent/AGENT.md` (this project)

- [ ] Task 4.3: Verify `/audit` is installed (not excluded)
  - Check the `rm -f` lines in `AGENT_SETUP_GUIDE.md` and `new-project.md`
  - Ensure `audit.md` is NOT in the excluded list

### Verification

- [ ] Read `audit.md` — all steps present, under 12K characters
- [ ] Available Commands consistent across all templates
- [ ] `/audit` not in excluded workflows list
- [ ] No hardcoded paths in new content

---

## Final Verification

- [ ] All acceptance criteria met
- [ ] All modified workflow files under 12K characters
- [ ] Run character count: `wc -c .agent/workflows/end-session.md .agent/workflows/status.md .agent/workflows/audit.md`
- [ ] Memory updated with decisions made
