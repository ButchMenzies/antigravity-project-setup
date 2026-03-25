# Plan: v12 Workflow & Skill Sync System

## Summary
Add `source: antigravity` tags to all distributed files, update the distribution logic to clean up leaked/stale files, bump to v12.

## Completion Criteria
- [ ] All distributed workflows and skills tagged with `source: antigravity`
- [ ] `update.md` includes cleanup step
- [ ] VERSION bumped to 12

## Phase 1: Tag All Distributed Files

### Tasks
- [ ] 1.1: Add `source: antigravity` to 21 distributed workflow frontmatters
- [ ] 1.2: Add `source: antigravity` to all skill SKILL.md frontmatters (22 in `skills/`, 2 in `.agent/skills/`)

### Verification
- [ ] Spot-check 3-4 files for valid YAML frontmatter

## Phase 2: Update Distribution Logic

### Tasks
- [ ] 2.1: Add Step 5b (Cleanup Stale Files) to `update.md` with three-category logic
- [ ] 2.2: Update `update.md` completion message for v12

### Verification
- [ ] Trace logic for code project with leaked `new-project.md` + `update-guide.md`
- [ ] Trace logic for workspace project
- [ ] Trace logic for project with user-created custom workflow

## Phase 3: Version Bump

### Tasks
- [ ] 3.1: Bump `VERSION` from 11 → 12
- [ ] 3.2: Update `AGENT_SETUP_GUIDE.md` version references to 12

### Verification
- [ ] End-to-end trace from end-session Step 9 → version mismatch → update.md → cleanup

## Final Verification
- [ ] All completion criteria met
- [ ] Memory updated with decisions made
