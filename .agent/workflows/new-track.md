---
description: Plan a new piece of work — creates spec and phased implementation plan
---

# New Track

Plan work for the next phase of the project. If a roadmap exists, this workflow picks up where you left off. Otherwise, it plans from scratch.

## Pre-flight

1. Read `.agent/AGENT.md` for project context and tech stack
2. Read `.agent/memory.md` for recent decisions and last session's log entry
3. **Read `.agent/skills/planning/SKILL.md`** — apply planning principles throughout this workflow
4. Check `.agent/skills/` for other skills relevant to this type of work
5. **If the track involves UI work**: check if `.agent/ux/` exists. If yes, read `persona.md` and `design-direction.md` to inform the spec. If no, suggest running `/ux-design` first.

---

## Step 1: Situational Awareness

Check for `conductor/roadmap.md`. If it exists, read it. Determine which state you're in:

### State A: Active phase is incomplete

A phase marked with 🔄 has tasks remaining.

```
I see Phase [N]: [Name] is still in progress.

Progress: [completed]/[total] items done.
Remaining:
- [list remaining items]

Options:
1. Continue this phase — I'll set up a track for the remaining work
2. Start a different phase from the roadmap
3. Work on something not on the roadmap
```

**Wait for user response.**

### State B: Previous phase complete, next phase available

The last 🔄 phase is done (or all items checked), and there's a next phase without ✅.

```
Phase [N-1]: [Name] is complete ✅
Next on the roadmap is Phase [N]: [Name]

It currently has these items:
- [list items from roadmap]

Options:
1. Plan this phase — let's flesh it out
2. Pick a different phase
3. Work on something not on the roadmap
```

**Wait for user response.**

### State C: No roadmap exists

Fall through to Step 2 (brainstorm gate) and Step 3 (interactive spec) as normal.

---

## Step 2: Brainstorm Gate

Before detailed planning, ask:

```
Do you want to brainstorm this phase first, or do you already know what needs to happen?

1. Let's brainstorm — run /brainstorm-lite inline
2. I know what we need — let's plan it
```

**Wait for user response.**

If brainstorm: run `/brainstorm-lite` inline. When it concludes, use the output to inform the spec below. Transition seamlessly — don't announce it.

---

## Step 3: Interactive Specification

**CRITICAL RULES:**
- Ask **ONE question per turn**
- Wait for user response before proceeding
- If the roadmap or brainstorm already answered a question, **skip it** — don't re-ask
- If the user already described the work, extract answers and confirm rather than re-asking

### Q1: What are we building?

Skip if the roadmap phase already defines this clearly. Otherwise:

```
Describe the feature in one sentence.
[If context exists]: "So we're building [X] — is that right?"
```

### Q2: Acceptance criteria

```
How will we know it's done? Key requirements:

Suggested based on [roadmap / brainstorm / description]:
1. [Criterion 1]
2. [Criterion 2]
3. [Criterion 3]

Add, remove, or modify?
```

### Q3: Technical approach

```
Any preferences on how to implement this?

1. I have a specific approach in mind: [describe]
2. Use your best judgment based on the codebase
3. Let's discuss options
```

### Q4: Scope boundaries

```
Anything explicitly OUT of scope?

1. [Suggest likely exclusions]
2. Nothing specific — use your judgment
```

Skip any question where the answer is already obvious from context. The goal is a **lighter Q&A** when the roadmap provides direction.

---

## Plan Generation

After gathering the spec, generate a phased implementation plan:

```markdown
# Implementation Plan: [Title]

## Spec Summary
[One paragraph from Q&A / roadmap / brainstorm]

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

## Plan Storage

**Always write the plan to disk.** Plans must survive across chat sessions.

Create the track folder under `conductor/tracks/`:

```
conductor/tracks/[phase-name]/
├── spec.md          # The spec from Q&A
└── plan.md          # The implementation plan
```

If no `conductor/` exists, write the plan to **`.agent/current-plan.md`**.

## Error Handling

- If user cancels mid-spec: Save what we have, offer to resume later
- If plan seems too large: Suggest splitting into multiple tracks
- If context is unclear: Ask clarifying questions rather than assuming
