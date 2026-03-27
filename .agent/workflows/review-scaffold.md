---
name: review-scaffold
version: 1
description: >-
  Deep review of agent scaffolding — workflows, skills, rules, and AGENT.md.
  Use periodically to evolve the project's tooling based on real usage patterns.
  Replaces the old /update workflow with bottom-up self-evolution.
source: antigravity
---

# Review Scaffold

Periodic deep review of this project's agent setup. Analyses what's working, what's stale, and what's missing — then proposes targeted improvements.

> **When to run:** When prompted by `/end-session` (after 5+ observations accumulate in `improvements.md`), or any time the user feels the scaffolding needs attention.

---

## Step 1: Read All Context (Silent)

Read everything. Do not narrate each file — just absorb.

```
.agent/AGENT.md
.agent/memory.md
.agent/improvements.md
.agent/skills-catalog.md (if exists)
```

```bash
ls .agent/workflows/*.md
ls .agent/skills/*/SKILL.md 2>/dev/null
```

Review the last 3-5 session entries in `memory.md` for patterns.

---

## Step 2: Systematic Analysis

Analyse each layer of the scaffold:

### Workflows
- Compare the Available Commands list in AGENT.md against actual files in `.agent/workflows/`
- Are there workflows listed that don't exist as files? (stale references)
- Are there workflow files that aren't listed in Available Commands? (undiscoverable)
- Are any workflows outdated — referencing files, patterns, or tools that no longer exist?

### Skills
- Check `.agent/skills/` contents against the skills-catalog.md
- Are there skills that have never been triggered? (possibly poorly described)
- Are there gaps — types of work the project does frequently but has no skill for?
- Do any skills still have the "First Use — Adapt This Skill" header? (unadapted)

### Rules (AGENT.md)
- Review `## Don't Be Lazy` — are there violations that keep happening? (need stronger rules)
- Review `## Core Rules` — are any rules outdated or irrelevant for this project?
- Review `## Project Principles` — do they still reflect how the project actually works?
- Check `### Terminology` — are there terms the team uses that aren't documented?

### Agent Behaviour (from improvements.md)
- Look for patterns in the "Agent Behaviour" category
- Repeated corrections → should become Don't Be Lazy rules
- Repeated questions → should become skills or rules

---

## Step 3: Present Findings + Ask for Input

Present your analysis as a structured summary:

```
## Scaffold Review — [Date]

### What's Working
- [Things that are effective and should stay]

### Issues Found
- [Stale references, gaps, mismatches]

### Patterns from improvements.md
- [Grouped observations with proposed actions]

### Recommendations
1. [Specific, actionable change]
2. [Specific, actionable change]
3. [Specific, actionable change]
```

Then ask:

> "Are there any workflows or skills you feel aren't performing well in this project? Anything that frustrates you or feels incomplete?"

**Wait for user response.** Their feedback adds to your findings.

---

## Step 4: Brainstorm Refinements

If the user flagged specific workflows or skills:
- Discuss what's wrong and what "better" looks like
- Propose specific rewrites or modifications
- Back-and-forth until both sides are satisfied

If no specific feedback, skip to Step 5.

---

## Step 5: Propose Concrete Changes

Summarise all proposed changes in a clear list:

```
## Proposed Changes

### Modify
- [ ] Workflow X — [what changes and why]
- [ ] Skill Y — [what changes and why]
- [ ] AGENT.md rule Z — [what changes and why]

### Create
- [ ] New skill: [name] — [purpose]
- [ ] New rule: [description]

### Remove
- [ ] Stale reference: [what and why]
```

**Wait for user approval before making changes.**

---

## Step 6: Implement Changes

Make all approved changes. For each change:
1. Edit the file
2. Verify the edit doesn't break references to other files
3. If creating a new skill, follow `.agent/workflows/create-skill.md` Step 5 conventions

---

## Step 7: Reconcile Available Commands

Compare AGENT.md's `## Available Commands` section against actual `.agent/workflows/` files:
- Add entries for any workflow files that aren't listed
- Remove entries for workflows that no longer exist as files
- Verify descriptions match what the workflows actually do

---

## Step 8: Clear Processed Items

Remove processed observations from `.agent/improvements.md`. Leave unprocessed items and add a comment noting the review date:

```markdown
<!-- Last reviewed: [DATE] -->
```

---

## Step 9: Template Feedback (Opt-In)

> "Did any of the improvements we just made feel like they'd be useful for all projects, not just this one? If so, I can prepare a change spec for the Antigravity template."

If yes, generate a summary of the changes that should be pushed upstream. Save to `.agent/scaffold-feedback.md` for the template maintainer to review.

If no, skip.

---

## Rules

- **Do the homework before asking questions.** Read and analyse everything before involving the user.
- **Be specific, not vague.** "Workflow X is stale" is not actionable. "Workflow X references `workspace-audit.md` which doesn't exist" is.
- **Preserve project-specific content.** Never overwrite Project Principles, Terminology, or custom Don't Be Lazy rules.
- **One review, one session.** Don't split this across multiple sessions.
