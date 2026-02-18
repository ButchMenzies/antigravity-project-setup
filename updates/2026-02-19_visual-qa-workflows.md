# Update: Visual QA Workflow System

**Date**: 2026-02-19  
**Scope**: 3 new workflows + 1 new skill for design capture, site recreation, and visual comparison

---

## What Changed

1. **New workflow** — `/capture-target` for exhaustive design data capture from live sites
2. **New workflow** — `/recreate-site` for building a site from scratch using captured data
3. **New workflow** — `/compare-site` for fixing an existing build against captured target data
4. **New skill** — `visual-qa` — shared engine for Section Briefs, code application, and gap analysis

---

## How to Apply

### Step 1: Copy new workflow files

```bash
cp ~/Desktop/Antigravity/Antigravity\ Project\ Setup/.agent/workflows/capture-target.md .agent/workflows/capture-target.md
cp ~/Desktop/Antigravity/Antigravity\ Project\ Setup/.agent/workflows/recreate-site.md .agent/workflows/recreate-site.md
cp ~/Desktop/Antigravity/Antigravity\ Project\ Setup/.agent/workflows/compare-site.md .agent/workflows/compare-site.md
```

### Step 2: Copy new skill

```bash
mkdir -p .agent/skills/visual-qa
cp ~/Desktop/Antigravity/Antigravity\ Project\ Setup/.agent/skills/visual-qa/SKILL.md .agent/skills/visual-qa/SKILL.md
```

### Step 3: Remove old workflow (if it exists)

```bash
rm -f .agent/workflows/design-audit.md
```

### Step 4: Add commands to AGENT.md

Open `.agent/AGENT.md` and add these lines under `## Available Commands`:

```markdown
- `/capture-target` — capture design data from a live site
- `/recreate-site` — rebuild a site from captured data
- `/compare-site` — fix an existing build against captured target data
```

### Step 5: Log the update in memory

Add to `.agent/memory.md` under Session Log:

```markdown
### 2026-02-19 Applied Visual QA workflow update
**Changes**: Added `/capture-target`, `/recreate-site`, `/compare-site` workflows and `visual-qa` skill. Removed old `/design-audit` workflow if present. These enable capturing design data from live sites and rebuilding or comparing against that data.
```

---

## What These Changes Do

| Change | Effect |
|--------|--------|
| `/capture-target` | Interactive loop for capturing CSS values, typography, colours, spacing from a live site via Dev Tools |
| `/recreate-site` | Full pipeline: stack selection → capture → build → verify |
| `/compare-site` | Compare existing build against target data, apply fixes, visual verification |
| `visual-qa` skill | Shared engine: builds Section Briefs from data, applies exact values to code, runs gap analysis |
