---
description: Wrap up a session — update memory, note progress, prepare for next time
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

Append a session log entry to the `## Session Log` table in `.agent/memory.md`:

```markdown
| [Date] | [Brief summary of what was accomplished] |
```

Then check if any of these need logging (append to the specific sections):
- Important decisions made? → Append under `## Key Decisions`
- Tricky problems solved? → Append under `## Lessons Learned`
- User preferences discovered? → Append as bullets under `## User Preferences`

### 4. Update Roadmap Progress

If `conductor/roadmap.md` exists:
- Mark completed tasks/items with `[x]` or checkboxes
- If all items in a phase are done, change the phase status emoji to ✅
- If a phase is actively in progress, ensure it has the 🔄 emoji
- If a new phase was started, move 🔄 to it

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
