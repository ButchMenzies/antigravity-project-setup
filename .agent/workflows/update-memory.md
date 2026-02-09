---
description: Log a decision, lesson, preference, or issue to memory
---

# Update Memory

Explicitly log something important to `.agent/memory.md`.

## Steps

### 1. Determine Entry Type

```
What would you like to log?

1. Decision — a choice made and why
2. Lesson — something learned the hard way
3. Preference — how you like things done
4. Issue — a recurring problem and its fix
5. General note
```

### 2. Gather Details

**For decisions:**
```
What was the decision?
What was the context?
What alternatives were considered?
Why this choice?
```

**For lessons:**
```
What happened?
What was the root cause?
How was it fixed?
How to prevent it next time?
```

**For preferences:**
```
What's the preference?
(e.g., "always use named exports", "prefer functional components", "run on port 5010")
```

**For issues:**
```
What's the recurring issue?
What are the symptoms?
What's the quick fix?
What's the proper fix?
```

### 3. Write Entry

Append to the appropriate section of `.agent/memory.md`:

```markdown
### [Date] [Title]
**Context**: [context]
**Decision/Lesson/Fix**: [detail]
**Rationale**: [why]
```

Update the session log table.

### 4. Confirm

```
✅ Logged to memory: [title]

→ Continue working
→ /status — see project overview
```
