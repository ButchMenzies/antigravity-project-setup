---
description: New conversation kickoff. Loads context, checks inbox for handoffs, briefs the user. Run this at the start of every fresh conversation.
---

# /start — New Conversation

Load context, check for handoffs, brief the user. This is how every conversation begins.

> **Every new conversation should start with `/start`.** If the user doesn't run it, suggest it.

## Rules

1. **Do everything silently.** Read files without narrating each one. The user gets the brief, not a play-by-play.
2. **Do not create or modify any files.** This is read-only.
3. **Do not start working on anything.** The brief ends with a question, not an action.
4. **Lead with what matters.** If there's nothing urgent, say so and move on.

## Steps

### 1. Load Core Context (Silent)

Read all of these. Don't announce them:

- `.agent/AGENT.md`
- `.agent/memory.md`
- `conductor/product.md` (if exists)
- `conductor/roadmap.md` (if exists)

### 2. Check for Active Work (Silent)

Check each — only mention what exists:

- `.agent/brainstorm-notes.md` — active brainstorm in progress?
- `.agent/current-plan.md` — pending plan awaiting execution?
- `conductor/tracks/` — any in-progress tracks?

### 3. Check Inbox

Read `.agent/inbox.md` if it exists. If there are uncompleted handoff items (unchecked boxes), present them to the user:

```
📬 **Inbox** — [N] handoff(s) from butchs-world

• [Date] — [Subject] ([N] action items)
  [Brief summary of what's requested]
```

If inbox is empty or all items are complete, skip this section.

### 4. Brief the User

Present a concise briefing. No filler, no padding, no menus. Format:

```
📋 **Brief**

[Last session]: [one-line summary of what was done and when]

[Active work]: [brainstorm in progress / plan pending / track active — or "Nothing active"]

[Inbox]: [handoff summary if any — or omit if empty]

[Anything that needs attention]: [overdue items, unfinished work — or omit if nothing]

What are we working on?
```

Keep it to 5-8 lines. Omit empty sections — don't write "Nothing" for every one.

### 5. Wait

That's it. Don't suggest slash commands. Don't list options. Don't start working.

The user will tell you what they want to do.
