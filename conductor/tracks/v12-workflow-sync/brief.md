# Brief: v12 Workflow & Skill Sync System

## Summary

Add `source: antigravity` frontmatter tags to all distributed workflows and skills, then update the distribution logic to automatically clean up leaked/stale files when user projects update. Triggered by bumping VERSION to 12.

## Completion Criteria

- All 21 distributed workflows have `source: antigravity` in frontmatter
- All distributed skills have `source: antigravity` in SKILL.md frontmatter
- `update.md` includes cleanup step that removes leaked files and flags unknown ones
- VERSION bumped to 12
- Bootstrapper (`AGENT_SETUP_GUIDE.md`) references v12

## Origin

Discovered that pre-v11 bootstrapper used `cp *.md` wildcard which leaked setup-only workflows (`new-project.md`, `update-guide.md`) to user projects. Current distribution logic is correct but doesn't clean up old leaks.

## Out of Scope

- Changing end-session.md (existing Step 9 version check is sufficient)
- Changing new-project.md distribution logic (already correct)
- Pushing updates to other projects (happens naturally when they next run end-session or paste bootstrapper)
