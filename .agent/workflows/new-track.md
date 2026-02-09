---
description: Plan a new piece of work — creates spec and phased implementation plan
---

# New Track

Create a spec and implementation plan for a new piece of work before coding begins.

## Pre-flight

1. Read `.agent/AGENT.md` for project context and tech stack
2. Check if `conductor/tracks/` exists — if so, read `conductor/tracks.md` for active tracks
3. Check `.agent/skills/` for skills relevant to this type of work

## Track Classification

Ask the user (or infer from their description):

```
What type of work is this?

1. Feature — new functionality
2. Bug — fix for an existing issue
3. Chore — maintenance, dependencies, config
4. Refactor — code improvement without behavior change
```

## Interactive Specification

**CRITICAL RULES:**
- Ask **ONE question per turn**
- Wait for user response before proceeding
- If the user already described the work, extract answers and confirm rather than re-asking

### For Features (max 5 questions)

**Q1: What are we building?**
```
Describe the feature in one sentence.
[If user already described it, confirm]: "So we're building [X] — is that right?"
```

**Q2: User story**
```
Who is this for and what do they need?

As a [user type], I want to [action] so that [benefit].

Suggested:
1. [Infer from description]
2. Type your own
```

**Q3: Acceptance criteria**
```
How will we know it's done? List the key requirements:

Suggested based on description:
1. [Criterion 1]
2. [Criterion 2]
3. [Criterion 3]

Add, remove, or modify?
```

**Q4: Technical approach**
```
Any preferences on how to implement this?

1. I have a specific approach in mind: [describe]
2. Use your best judgment based on the codebase
3. Let's discuss options
```

**Q5: Scope boundaries**
```
Anything explicitly OUT of scope?

1. [Suggest likely exclusions based on feature]
2. Nothing specific — use your judgment
```

### For Bugs (max 4 questions)

**Q1: What's broken?**
```
Describe the bug. What should happen vs what actually happens?
```

**Q2: How to reproduce**
```
Steps to reproduce:
1. [Infer from description or ask]
```

**Q3: Affected areas**
```
What parts of the system are affected?
[Suggest based on description]
```

**Q4: Root cause hypothesis**
```
Any idea what's causing this? (Skip if unsure)
```

### For Chores/Refactors (max 3 questions)

**Q1: What needs to be done?**
```
Describe the maintenance task or improvement.
```

**Q2: Why now?**
```
What's the motivation?
1. Technical debt is slowing us down
2. Dependency updates needed
3. Performance improvement
4. Code quality / readability
```

**Q3: Constraints**
```
Any constraints or risks?
1. Must not break existing functionality
2. Stay within current architecture
3. [Other]
```

---

## Plan Generation

After gathering the spec, generate a phased implementation plan:

```markdown
# Implementation Plan: [Title]

## Spec Summary
[One paragraph from Q&A answers]

## Acceptance Criteria
- [ ] [From Q&A]

## Phase 1: [Phase Name]

### Tasks
- [ ] Task 1.1: [Description]
- [ ] Task 1.2: [Description]

### Verification
- [ ] [How to verify this phase works]

## Phase 2: [Phase Name]

### Tasks
- [ ] Task 2.1: [Description]
- [ ] Task 2.2: [Description]

### Verification
- [ ] [How to verify this phase works]

## Final Verification
- [ ] All acceptance criteria met
- [ ] Tests passing (if applicable)
- [ ] Memory updated with decisions made
```

### Plan Guidelines

- Group related tasks into logical phases (2-4 phases typical)
- Each phase should be independently verifiable
- First phase should be the smallest meaningful unit
- Include test tasks if the project uses testing
- Keep tasks concrete — each should be a specific code change

---

## Present Plan for Approval

```
Here's the plan for: [Title]

[Show the full plan]

What would you like to do?
→ /implement — start building this plan
→ /edit — review or change the plan first
```

## Track Storage

If `conductor/tracks/` exists, create:

```
conductor/tracks/[trackId]/
├── spec.md          # The spec from Q&A
├── plan.md          # The implementation plan
└── metadata.json    # Track metadata
```

And add entry to `conductor/tracks.md`.

If no conductor setup, store the plan in the conversation context and reference it during `/implement`.

## Error Handling

- If user cancels mid-spec: Save what we have, offer to resume later
- If plan seems too large: Suggest splitting into multiple tracks
- If context is unclear: Ask clarifying questions rather than assuming
