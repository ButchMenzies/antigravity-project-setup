# Update: Skills Catalogue + Honesty Rule

**Date**: 2026-02-16  
**Scope**: Workflow improvements + new core rule

---

## What Changed

1. **New Core Rule** — "Don't guess about tools, settings, or platform behaviour"
2. **Superpowers library** added as a skill discovery source
3. **Auto-sync** — skills created in any project now sync to the central catalogue
4. **End-session scoping** — memory updates are scoped to the current project only

---

## How to Apply

### Step 1: Update workflows from the central repo

```bash
cp ~/Desktop/Antigravity/Antigravity\ Project\ Setup/.agent/workflows/create-skill.md .agent/workflows/create-skill.md
cp ~/Desktop/Antigravity/Antigravity\ Project\ Setup/.agent/workflows/end-session.md .agent/workflows/end-session.md
```

### Step 2: Add Core Rule 6 to AGENT.md

Open `.agent/AGENT.md` and add this as item 6 under `## ⚠️ Core Rules (Always Apply)`:

```markdown
6. **Don't guess about tools, settings, or platform behaviour.** If you're unsure how something works — especially IDE features, APIs, or config options — say so and verify first. Trust your coding knowledge; verify everything else.
```

### Step 3: Log the update in memory

Add to `.agent/memory.md` under Session Log:

```markdown
### 2026-02-16 Applied skills catalogue + honesty rule update
**Changes**: Added Core Rule 6 (don't guess about tools/platform). Updated create-skill.md (Superpowers discovery + central catalogue sync). Updated end-session.md (project-scoped memory).
```

---

## What These Changes Do

| Change | Effect |
|--------|--------|
| Core Rule 6 | Agent says "I'm not sure" instead of guessing about platform/tool behaviour |
| create-skill.md Step 1d | Searches obra/superpowers for development workflow skills |
| create-skill.md Step 8a | Copies new skills to central catalogue at `~/Desktop/Antigravity/Antigravity Project Setup/skills/` |
| end-session.md scoping | Prevents agent from logging other projects' work into this project's memory |
