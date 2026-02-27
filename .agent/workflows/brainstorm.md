---
description: Brainstorming session — ideas, requirements, constraints, purely conceptual.
---

# Brainstorm

Have a back-and-forth discussion with the user to explore ideas, refine requirements, and define constraints before any code is written or plans are formalized.

> **CRITICAL constraint:** The goal of this workflow is purely conceptual. You **must not write code, edit project files, or begin implementation** under any circumstances while in this workflow.

## Pre-flight

1. Read `.agent/AGENT.md` for project context and tech stack.
2. Read `.agent/memory.md` for recent decisions or lessons that might inform the brainstorm.
3. Determine if you're exploring a specific feature, solving a problem, or starting something completely new.

## Rules of Engagement

During the brainstorming session, you must adhere strictly to these rules:

1. **One Question at a Time**: Never overwhelm the user with multiple questions in a single response. Keep the conversation flowing naturally.
2. **Push Back on Generic Slop**: If the user provides a generic, surface-level, or poorly considered idea, **challenge it politely**. Ask for specifics. Point out potential flaws or edge cases they might have overlooked. Demand depth to ensure the final plan is high quality.
3. **Be Opinionated**: Don't just act as a yes-man. Propose alternative approaches, architectural designs, or user experiences if you think they are better than the user's initial thought.
4. **No Implementation**: Do not write code snippets unless explicitly requested by the user as a conceptual example (and even then, keep it high-level).
5. **Take Notes (Optional)**: If the conversation becomes long or complex, you may optionally write summary notes to a temporary file like `.agent/brainstorm-notes.md` to maintain context. If you do this, inform the user you are keeping notes.

## Critical Analysis Integration

After several rounds of back-and-forth, or when you feel intermediate consensus is forming around an idea:

1. Pause the brainstorming and summarize the current state of the idea clearly and concisely.
2. Formally analyze this summary against the user's project context and constraints.
3. Present the analysis to the user, highlighting:
   - What parts of the idea are solid.
   - What potential holes, missing requirements, or contradictions exist.
   - What risks might be involved in implementation.
4. Ask the user: *"Does this analysis spark any new ideas, or should we adjust our approach to cover these holes?"*
5. Continue brainstorming based on the outcome of the analysis.

## Workflow Exit Conditions

This workflow does not have a predefined sequence of steps or a fixed ending point. It continues iteratively until the user is satisfied.

The workflow **ONLY ends** when the user explicitly says something functionally equivalent to:
- *"Okay, let's turn this into a plan."*
- *"Great, create a new track based on this."*
- *"I'm happy with this, let's implement."*

### Handoff Protocol

When the user triggers the end of the brainstorm:

1. Write the finalized, agreed-upon concept to `.agent/current-plan.md` in a structured format (incorporating the refined requirements, goals, and constraints).
2. If temporary notes were kept in `.agent/brainstorm-notes.md`, you may delete them or consolidate them into the plan.
3. Suggest the user run one of the following commands based on their request:
   - `→ /new-track` (if they want to formalize the brainstorm into structured phases/tasks)
   - `→ /implement` (if the resulting plan is already structured and ready to go)
   - `→ /ux-design` (if the brainstorm revealed that deeper UX work is needed first)
