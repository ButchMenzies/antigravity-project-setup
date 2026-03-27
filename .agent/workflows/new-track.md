---
name: new-track
version: 3
description: >-
  Plans a new piece of work by creating a brief and phased plan.
  Use when starting a new feature, refactor, or any scoped piece of work.
source: antigravity
---

# New Track

Plan work for the next phase of the project. If a roadmap exists, this workflow picks up where you left off. Otherwise, it plans from scratch.

## Pre-flight

1. Read `.agent/AGENT.md` for project context
2. Read `.agent/memory.md` for recent decisions and last session's log entry
3. **Read `.agent/skills/planning/SKILL.md`** — apply planning principles throughout this workflow
4. **Scan `.agent/skills/` for matching skills.** For each skill with a `SKILL.md`, read its frontmatter. A skill matches if:
   - Its `track_types` includes the track type from classification (feature, bug, chore, refactor)
   - OR its `triggers` match keywords in the user's description
   - Read the full `SKILL.md` of each matching skill — you'll incorporate their **Plan Checklist** items into the implementation plan
   - Typically 1-3 skills will match. If none match, proceed normally
5. **If the track involves UI or content work**: check if `.agent/ux/` or `brand/` exists. If yes, read `persona.md` and `design-direction.md` to inform the brief. If no, suggest running `/ux-design` (code projects) or `/brand-design` (workspace projects) first.

---

## Step 1: Situational Awareness

Check for `conductor/roadmap.md`. If it exists, read it. Determine which state you're in:

### State A: Active Track exists and is incomplete

The `## Active Track` section has work defined.

```
I see an active track: [Name]

Progress:
- [list completed and remaining items]

Options:
1. Continue this track — I'll set up the remaining work
2. Start something different from the backlog
3. Work on something not on the roadmap
```

**Wait for user response.**

### State B: No active track, backlog available

The Active Track section is empty but the backlog has themes.

```
No active track. Here are the backlog themes:

1. [Theme 1] — [N] items
2. [Theme 2] — [N] items
...

Options:
1. Pick a theme to work on
2. Work on something not on the roadmap
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

## Step 2b: Codebase Reconnaissance

Re-read `.agent/AGENT.md` — specifically `## Your Role` and `## Don't Be Lazy`.

If `.agent/skills/architecture-change/SKILL.md` exists, read its "Before Proposing Changes" section.
Apply the reconnaissance steps to understand what exists before specifying what to build.

If no architecture-change skill exists, do reconnaissance manually:
- **Map the relevant data flows** — trace how related features currently work
- **Identify affected modules** — what will this new work need to touch?
- **Check the data layer** — what tables/models/schemas exist for this area?

Summarise findings to the user before proceeding to the spec.

---

## Step 3: Interactive Brief

**CRITICAL RULES:**
- Ask **ONE question per turn**
- Wait for user response before proceeding
- If the roadmap or brainstorm already answered a question, **skip it** — don't re-ask
- If the user already described the work, extract answers and confirm rather than re-asking

### Q1: What are we working on?

Skip if the roadmap phase already defines this clearly. Otherwise:

```
Describe the work in one sentence.
[If context exists]: "So we're working on [X] — is that right?"
```

### Q2: Completion criteria

```
How will we know it's done? Key requirements:

Suggested based on [roadmap / brainstorm / description]:
1. [Criterion 1]
2. [Criterion 2]
3. [Criterion 3]

Add, remove, or modify?
```

### Q3: Approach

```
Any preferences on how to approach this?

1. I have a specific approach in mind: [describe]
2. Use your best judgment based on the project
3. Let's discuss options
```

### Q4: Scope boundaries

```
Anything explicitly OUT of scope?

1. [Suggest likely exclusions]
2. Nothing specific — use your judgment
```

Skip any question where the answer is already obvious from context. The goal is a **lighter Q&A** when the roadmap or brainstorm provides direction. For workspace projects, Q3 (approach) can often be skipped entirely.

---

## Plan Generation

After gathering the brief, generate a phased plan:

```markdown
# Plan: [Title]

## Summary
[One paragraph from Q&A / roadmap / brainstorm]

## Completion Criteria
- [ ] [From Q&A]

## Phase 1: [Phase Name]

### Tasks
- [ ] Task 1.1: [Description]
- [ ] Task 1.2: [Description]

### Verification
- [ ] [How to verify this phase is complete]

## Phase 2: [Phase Name]

### Tasks
- [ ] Task 2.1: [Description]
- [ ] Task 2.2: [Description]

### Verification
- [ ] [How to verify this phase is complete]

## Final Verification
- [ ] All completion criteria met
- [ ] Tests passing (if applicable)
- [ ] Memory updated with decisions made
```

### Plan Guidelines

- Group related tasks into logical phases (2-4 phases typical)
- Each phase should be independently verifiable
- First phase should be the smallest meaningful unit
- Include test tasks if the project uses testing
- Keep tasks concrete — each should be a specific deliverable or change
- **Incorporate matched skill checklists:** For each skill matched in pre-flight step 4, weave its **Plan Checklist** items into the relevant plan phases. Don't dump them as a separate section — integrate them naturally. For example, a database-change skill's "RLS Policies" items go into the phase that handles database work, not into a standalone "skill checklist" phase
- **Don't over-load small tracks:** If the track is a quick bug fix or minor chore, only include skill items that are genuinely relevant. A one-line CSS fix doesn't need a full component reuse scan

---

## Present Plan for Approval

```
Here's the plan for: [Title]

[Show the full plan]

What would you like to do?
→ /implement — start executing this plan
→ /edit — review or change the plan first
```

## Plan Storage

**Always write the plan to disk.** Plans must survive across chat sessions.

Create the track folder under `conductor/tracks/`:

```
conductor/tracks/[track-name]/
├── brief.md         # The brief from Q&A
└── plan.md          # The plan
```

If no `conductor/` exists, write the plan to **`.agent/current-plan.md`**.

## Error Handling

- If user cancels mid-spec: Save what we have, offer to resume later
- If plan seems too large: Suggest splitting into multiple tracks
- If context is unclear: Ask clarifying questions rather than assuming
