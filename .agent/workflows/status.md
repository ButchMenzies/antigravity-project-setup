---
name: status
version: 2
description: >-
  Shows project status including active tracks, recent memory, and
  available skills. Use when checking what's in progress or getting
  oriented in a project.
source: antigravity
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


### 2. Roadmap Overview

If `conductor/roadmap.md` exists:
- Read it and show current status:

```
🗺️ Roadmap

Active Track: [Name or "None"]
[If active: brief progress summary]

Backlog: [N] themes, [M] total items
Top themes: [list top 3 themes by priority]
```

If `conductor/notes` exists with content beyond the header:
```
📝 [N] pending notes in conductor/notes
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

Read `## Recent Sessions` from `.agent/memory.md`:
```
🧠 Recent Sessions

- [Date]: [summary]
- [Date]: [summary]
- [Date]: [summary]
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
