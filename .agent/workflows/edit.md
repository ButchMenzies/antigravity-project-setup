---
description: Revise or refine an implementation plan before proceeding
---

# Edit Plan

Structured plan revision. Use this when a plan needs changes before implementing.

## Pre-flight

1. Locate the active plan:
   - Check `conductor/tracks/` for the most recent or active track
   - If no tracks, use the plan from conversation context
2. Display the full plan to the user

## Revision Flow

### 1. Show Current Plan

```
Here's the current plan for: [Title]

[Display full plan with phase/task numbering]

What would you like to change?

1. Add tasks or phases
2. Remove tasks or phases
3. Modify existing tasks
4. Reorder phases
5. Change scope / acceptance criteria
6. Start over with a fresh plan
```

### 2. Gather Changes

Based on user's choice, ask targeted questions:

**Adding tasks:**
```
Where should the new task go?
- After Task [X.Y]
- As a new phase at the end
- As a new phase before Phase [N]

Describe the task:
```

**Removing tasks:**
```
Which tasks should be removed?
[List tasks with numbers for easy selection]
```

**Modifying tasks:**
```
Which task needs changes?
[Show task] → What should it be instead?
```

**Changing scope:**
```
What's changing about the scope?
Current acceptance criteria:
- [List criteria]

Add, remove, or modify?
```

### 3. Present Updated Plan

Show the revised plan with changes highlighted:

```
Updated plan:

[Show full plan]

Changes made:
- Added: [list additions]
- Removed: [list removals]
- Modified: [list modifications]

Happy with this?
1. Yes — ready to implement
2. More changes needed
```

If more changes: loop back to step 2.

### 4. Save Changes

Update `plan.md` in conductor tracks (if they exist).

```
Plan updated.

→ /implement — start building this plan
→ More changes? Just describe what you want to adjust.
```
