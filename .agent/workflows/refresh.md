---
name: refresh
version: 1
description: >-
  Mid-conversation context reset. Re-reads all project files, summarises
  progress, and resets focus. Use when context feels stale, after long
  conversations, or when switching between tasks.
source: antigravity
---

# Refresh

Stop, re-read everything, and reset your context. No files are created or modified.

> **Use this when** the conversation is getting long and you feel you might be losing context, or when switching between different tasks within the same session.

## Steps

### 1. Summarise Progress

Before re-reading, capture what's happened in this conversation so far:
- What was the user's original request?
- What has been accomplished?
- What is currently in progress or pending?

State this summary to the user in 3-5 bullet points.

### 2. Re-Read Project Context

Read all of these files (silently — don't narrate each one):

```
.agent/AGENT.md
.agent/memory.md
.agent/improvements.md (if exists)
.agent/current-plan.md (if exists)
.agent/brainstorm-notes.md (if exists)
conductor/product.md (if exists)
conductor/roadmap.md (if exists)
```

Also check for any active track:
```bash
ls conductor/tracks/*/plan.md 2>/dev/null | head -5
```

If an active track plan exists, read it.

### 3. State Current Focus

After re-reading, tell the user:
- What the current task or objective is
- What the next step should be
- Any context you picked up that changes your approach

Then proceed with the work.

## Rules

- **Do not create or edit any files.** This is a read-only operation.
- **Do not announce each file as you read it.** Read silently, then summarise.
- **Be concise.** The summary and focus statement should be 5-10 lines total, not a page.
