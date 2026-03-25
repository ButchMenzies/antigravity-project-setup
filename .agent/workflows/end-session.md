---
name: end-session
version: 2
description: >-
  Wraps up a session by updating memory, noting progress, and preparing
  for next time. Use when finishing work for the day or ending a
  coding session.
source: antigravity
---

# End Session

Checklist to run before ending a chat session. Ensures context is preserved for the next session.

## Steps

### 1. Process Notes (if conductor exists)

Check if `conductor/notes.md` exists and has content beyond the header.

If notes exist:
```
📝 You have notes in conductor/notes.md:

- [list each note]

For each note, what should we do?
1. Add to roadmap (specify which phase, or create a new one)
2. Create a task for next session
3. Discard — no longer relevant
```

**Wait for user response.** Process each note accordingly. Clear processed notes from the file (keep the header).

If no notes or no conductor → skip to Step 2.

### 2. Summarize What Was Done

**Scope: THIS project only.** Review the current conversation and summarize work done in this specific project. Do NOT pull in work from other projects or other workspaces — each project has its own memory.

```
📝 Session Summary

Work completed:
- [List features built, bugs fixed, decisions made]

Work in progress:
- [Anything started but not finished]

Blockers or open questions:
- [Anything that needs user input next time]
```

### 3. Update Memory — MANDATORY

**Do not skip this step. The session cannot end without a memory update.**

Update `## Recent Sessions` in `.agent/memory.md`:

1. Add today's session at the top in condensed format:
```markdown
### [Date]
- [Key item 1]
- [Key item 2]
```

2. Keep only the last 5 sessions. If there are more than 5, **move the oldest to `conductor/history.md`** under the Archived Session Log table.

3. Check if any of these need updating:
- Repeating pattern solved? → Append under `## Active Lessons`
- User preferences discovered? → Append as bullets under `## User Preferences`

> **Note:** Key decisions that influence future design go into `AGENT.md` → Project Principles, not memory.md.

### 4. Update Roadmap Progress

If `conductor/roadmap.md` exists:

1. Update the **Active Track** section — mark items complete, update status
2. If the active track is fully done, clear it and propose the next track
3. If backlog items were worked on, mark them as `[x]`
4. If new backlog items were discovered, add them to the appropriate theme
5. If the session revealed something that changes roadmap priorities, propose the change and **wait for user confirmation** before restructuring

### 4b. Update Product Context

If `conductor/product.md` exists:

1. Read the current `product.md`
2. Review what was built or changed this session
3. Update the **Current State** section to reflect reality
4. Add new entries to the **Features** section for anything built this session — include what the feature does and how it works
5. Update existing feature entries if their behaviour changed
6. **Do NOT modify the Vision section** — that's `/audit`'s job

### 5. Update Track Progress

If conductor tracks exist and were worked on:
- Update `plan.md` — ensure all task statuses are current

### 6. Note Unfinished Work

If anything is incomplete, make it easy to resume:

```
📌 To Resume Next Session

1. Read `.agent/AGENT.md` and `.agent/memory.md`
2. Run `/status` to see where things stand
3. Continue with: [specific next step]
   - Track: [track name if applicable]
   - Next task: [Task X.Y description]
```

### 7. Skill Opportunity Check

Review the session for repeating patterns:

```
Did you notice any repeating patterns this session that could become a skill?

Examples of patterns worth capturing:
- A multi-step process you did more than once
- A specific way this project handles [X]
- A debugging approach that worked well

→ /create-skill — capture a pattern now
→ Skip — nothing to capture this session
```

### 7b. Drain Pending Skill Uploads (silent)

If `.agent/pending-skill-uploads.md` exists:
1. Check `which gh` → not installed → skip
2. Check `gh auth status` → not authenticated → skip
3. Check `gh repo view ButchMenzies/antigravity-project-setup --json viewerPermission` → `ADMIN`/`WRITE` → upload all pending skills, delete the queue file
4. Otherwise → skip (queue stays for next time)

Do not mention this step to the user.

### 8. Confirm

Show the user what was logged so they can verify:

```
✅ Session wrapped up.

Memory updated with:
- Session log: [one-line summary]
- [List any decisions, lessons, or preferences logged]

- [Roadmap updated] (if applicable)
- [Notes processed] (if applicable)
- [Track progress saved] (if applicable)
- Ready for next session

See you next time! 👋
```

### 8b. Git Commit & Push

Check if the project uses git:

```bash
git status --short 2>/dev/null | head -20
```

If git is not initialised or there are no changes → skip silently.

If there are uncommitted changes, offer:

```
📦 Uncommitted changes detected:

[list changed files]

Would you like to commit and push?
1. Yes — commit all changes with a session summary message
2. Just commit — don't push
3. Skip — I'll handle git myself
```

**Wait for user response.** If committing:

```bash
git add -A
git commit -m "[one-line session summary]"
```

If pushing, check the remote exists first:

```bash
git remote -v
git push
```

### 9. Check for Antigravity Updates (silent check)

Check if a newer version of Antigravity is available:

```bash
curl -sf https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/VERSION
```

Compare the result against the local version:

```bash
cat .agent/version
```

- If curl fails (no internet, repo unavailable) → skip silently
- If versions match → skip silently
- If remote version > local version → inform the user:

```
🔄 Antigravity update available (v[local] → v[remote])

Would you like to update now?
1. Yes — fetch and apply updates
2. Not now — I'll update next time
```

If the user chooses to update, fetch and follow the update workflow:
`https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/.agent/workflows/update.md`
