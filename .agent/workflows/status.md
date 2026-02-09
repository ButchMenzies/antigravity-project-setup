---
description: Show project status — active tracks, recent memory, available skills
---

# Project Status

Quick overview of the project's current state.

## Steps

### 1. Project Context

Read and summarize `.agent/AGENT.md`:
- Project name and description
- Tech stack

### 2. Active Tracks

If `conductor/tracks/` exists:
- Read `conductor/tracks.md`
- For each active track, show:

```
📋 Active Tracks

[~] [Track Title] — [type]
    Phase 2/3 | Tasks: 4/7 complete
    Next: Task 2.3 — [description]

[ ] [Track Title] — [type]
    Not started
    
[x] [Track Title] — [type]
    Completed [date]
```

If no conductor tracks:
```
No tracks set up. Run /new-track to plan work.
```

### 3. Recent Memory

Read the last 3 entries from `.agent/memory.md`:
```
🧠 Recent Memory

- [Date] [Decision/Lesson]: [summary]
- [Date] [Decision/Lesson]: [summary]
- [Date] [Decision/Lesson]: [summary]
```

### 4. Local Skills

List `.agent/skills/`:
```
🛠️ Local Skills

- create-skill — Create new project-local skills
- [any others created]
```

### 5. Suggested Actions

Based on current state, suggest next steps:

```
💡 Suggested Next

→ /new-track — to start a new piece of work
→ /implement — to continue [active track name]
→ /update-memory — to log something important
```
