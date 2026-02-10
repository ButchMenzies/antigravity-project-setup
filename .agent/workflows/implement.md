---
description: Execute a plan with progress tracking, memory checkpoints, and skill prompts
---

# Implement

Execute tasks from an implementation plan with structured tracking.

## Pre-flight

1. Read `.agent/AGENT.md` for project context
2. **Read `.agent/memory.md`** for relevant history — do not skip this
3. **Scan `.agent/skills/`** — read the SKILL.md of any skill relevant to the current work
4. Locate the active plan:
   - Check `.agent/current-plan.md` first
   - Then check `conductor/tracks/` for track plans (read `tracks.md` to find active track)
   - If no plan files, check conversation context for the most recent plan
5. If no plan found: suggest running `/new-track` first

## Track Selection (if multiple tracks exist)

```
Which track do you want to implement?

In Progress:
1. [~] [Track title] — Phase 2/3, Task 2.1 next

Pending:
2. [ ] [Track title] — Not started

Select a number, or describe what you want to work on:
```

## Task Execution Loop

For each incomplete task in the plan:

### 1. Announce the Task

```
Starting Task X.Y: [Description]
Phase: [Phase Name] ([completed]/[total] tasks in this phase)
```

### 2. Mark In-Progress

Update `plan.md` (if using conductor tracks):
- Change `- [ ] Task X.Y` to `- [~] Task X.Y`

### 3. Execute the Work

Do the actual implementation. Follow these rules:

- **Read before writing** — understand existing code before changing it
- **Small changes** — make incremental, testable changes
- **Test as you go** — verify each change works before moving on
- **Follow project conventions** — match existing patterns in the codebase

### 4. Verify the Task

Before marking complete:
- Does the change work as intended?
- Are there any obvious side effects?
- If tests exist, do they pass?

### 5. Mark Complete

Update `plan.md`:
- Change `- [~] Task X.Y` to `- [x] Task X.Y`

### 6. Phase Completion Check

After each task, check if the current phase is complete (all tasks `[x]`).

If phase complete:

```
✅ Phase [N] complete: [Phase Name]

Completed tasks:
- [x] Task N.1: [Description]
- [x] Task N.2: [Description]

Phase verification:
- [ ] [Run verification steps from plan]
```

**Run verification steps.** If verification fails, fix before moving on.

### 6a. Re-evaluate the Next Phase (BEFORE proceeding)

Before moving on, review and refine the upcoming phase:

1. Re-read the next phase's outline in the plan
2. Consider what you learned during the phase you just completed
3. Ask: Does the original plan for the next phase still make sense?
4. Refine the next phase into concrete, actionable tasks
5. Update the plan file with the refined tasks
6. If the next phase has changed significantly, briefly tell the user what changed and why

**Do not skip this.** Plans written before execution are guesses. Plans refined after execution are informed.

### ⚠️ MEMORY CHECKPOINT (after each phase) — MANDATORY, DO NOT SKIP

**You must complete this before moving to the next phase.** This is not optional.

1. Update `.agent/memory.md`:
   - **Decisions** → append under `## Key Decisions` using the format in the section comments
   - **Lessons/gotchas** → append under `## Lessons Learned` using the format in the section comments
   - **Session log** → append a row to the `## Session Log` table

2. If you notice you've done a similar task pattern multiple times:
   ```
   I've noticed a repeating pattern: [describe pattern].
   
   Want me to create a local skill for this?
   → /create-skill — capture this pattern for future use
   → Skip — continue with implementation
   ```

3. **Do not proceed to the next phase until memory.md has been updated.**

### 7. Continue to Next Task

Proceed to the next incomplete task. Repeat the loop.

---

## Track Completion

When all phases and tasks are complete:

```
✅ Track complete: [Title]

Summary:
- Phases completed: [N]/[N]
- Tasks completed: [M]/[M]

All acceptance criteria:
- [x] [Criterion 1]
- [x] [Criterion 2]

Final verification:
- [ ] All features working as expected
- [ ] No regressions introduced
- [ ] Tests passing (if applicable)
```

Run final verification. Then:

```
What's next?
→ /end-session — wrap up and log this work
→ /new-track — start the next piece of work
→ /update-memory — log any final decisions or lessons
```

## Update Track Status

If using conductor tracks, update `metadata.json`:
- Set status to `complete`
- Update timestamps
- Update `conductor/tracks.md` to mark track as complete

---

## Error Handling

### If a task fails

```
❌ Task X.Y failed: [description of failure]

Options:
1. Attempt to fix
2. Skip this task and continue
3. Stop implementation — let's reassess the plan
```

### If scope creep is detected

If the work is expanding beyond the original plan:

```
⚠️ This task seems to be growing beyond the original scope.

The plan was: [original scope]
This is becoming: [expanded scope]

Options:
1. Continue — it's a reasonable expansion
2. Finish current task, then /edit the plan
3. Create a new track for the extra work
```

### If the agent is uncertain

```
I'm unsure about the best approach for Task X.Y.

Options I see:
1. [Approach A] — [pros/cons]
2. [Approach B] — [pros/cons]

Which direction do you prefer?
```

**Never proceed with implementation when uncertain without asking the user.**
