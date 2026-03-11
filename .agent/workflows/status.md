---
version: 1
description: Show project status — active tracks, recent memory, available skills
---

# Project Status

Quick overview of the project's current state.

## Steps

### 1. Project Context

Read and summarize `.agent/AGENT.md`:
- Project name and description
- Tech stack

### 1b. Product Context

If `conductor/product.md` exists:
- Read it and show a brief summary of Current State and feature count:

```
📦 Product

Current State: [one-line summary from Current State section]
Features built: [N] ([list feature names])
Vision items remaining: [N]
```

### 1c. Tech Stack Details

If `conductor/tech-stack.md` exists:
- Read it and show key decisions alongside the AGENT.md tech stack:

```
🔧 Tech Stack

[Core stack summary from tech-stack.md]
Recent decisions: [list any entries from the Decisions section]
```

### 2. Roadmap Overview

If `conductor/roadmap.md` exists:
- Read it and show phase-level summary:

```
🗺️ Roadmap

✅ Phase 1: [Name] — complete
🔄 Phase 2: [Name] — in progress ([X]/[Y] items)
   Phase 3: [Name] — upcoming
   Phase 4: [Name] — upcoming
```

If `conductor/notes.md` exists with content beyond the header:
```
📝 [N] pending notes in conductor/notes.md
```

### 3. Active Tracks

If `conductor/tracks/` exists:
- Read the active track's `plan.md` and `spec.md`
- For each active track, show:

```
📋 Active Tracks

[~] [Track Title] — [type]
    Phase 2/3 | Tasks: 4/7 complete
    Next: Task 2.3 — [description]
    Acceptance: 2/5 criteria met

[ ] [Track Title] — [type]
    Not started
    
[x] [Track Title] — [type]
    Completed [date]
```

If no conductor tracks:
```
No tracks set up. Run /new-track to plan work.
```

### 4. Recent Memory

Read the last 3 entries from `.agent/memory.md`:
```
🧠 Recent Memory

- [Date] [Decision/Lesson]: [summary]
- [Date] [Decision/Lesson]: [summary]
- [Date] [Decision/Lesson]: [summary]
```

### 5. Local Skills

List `.agent/skills/`:
```
🛠️ Local Skills

- create-skill — Create new project-local skills
- [any others created]
```

### 6. Suggested Actions

Based on current state, suggest next steps:

```
💡 Suggested Next

→ /new-track — to start a new piece of work
→ /implement — to continue [active track name]
→ /update-memory — to log something important
```
