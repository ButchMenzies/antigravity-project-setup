# Antigravity Setup Update — 2026-02-09T12:55

> **What**: Local-first skills architecture
> **Why**: Instead of searching a massive 626-skill global library, agents now create project-specific skills locally and use the global library only for reference/inspiration.
> **Impact**: Changes to `.agent/` folder structure, `skills-catalog.md`, and AGENT.md skill references.

---

## Instructions

Paste this entire document into the chat of any existing Antigravity project to apply this update.

**Agent: Follow these steps in order.**

---

### Step 1: Create the skills directory

```bash
mkdir -p .agent/skills/create-skill
```

### Step 2: Copy the create-skill meta-skill

```bash
cp ~/Desktop/Antigravity/Antigravity\ Project\ Setup/.agent/skills/create-skill/SKILL.md .agent/skills/create-skill/SKILL.md
```

Verify:
```bash
head -5 .agent/skills/create-skill/SKILL.md
```

### Step 3: Update skills-catalog.md

Open `.agent/skills-catalog.md` and restructure it to this format:

```markdown
# Skills Catalog

> **Local skills first, global library for reference.**

---

## Local Skills

Skills created specifically for this project, stored in `.agent/skills/`.

| Skill | Purpose |
|-------|---------|
| `create-skill` | Create new project-local skills from repeating patterns |

<!-- Add new local skills here as they are created -->

---

## How to Use Skills

1. **Check local skills first** — `ls .agent/skills/`
2. **If none exists** — consider creating one with the `create-skill` skill
3. **For inspiration** — search the global library (see below)
4. **Read the `SKILL.md`** before applying any skill

---

## Global Skills Reference

The global library is a **reference/inspiration source** — not the primary skill store.

### Searching
~~~bash
grep -i "<keyword>" ~/Desktop/Antigravity/antigravity-awesome-skills-main/skills_index.json | head -10
~~~

### Useful Global Skills by Category

| Category | Skills |
|----------|--------|
| **Architecture** | `brainstorming`, `architecture`, `senior-architect` |
| **Development** | `typescript-expert`, `react-patterns`, `api-design-principles` |
| **Testing** | `testing-patterns`, `test-driven-development` |
| **Infrastructure** | `docker-expert`, `vercel-deployment` |
| **Conductor** | `conductor-setup`, `conductor-new-track`, `conductor-implement` |
```

### Step 4: Update AGENT.md skill references

In `.agent/AGENT.md`, find and update the "Before Major Work" section (if it exists) to:

```markdown
## 💡 Before Major Work
Check `.agent/skills/` for a relevant local skill first.
If none exists, consider creating one — run the `create-skill` skill.
For inspiration, search the global library: `~/Desktop/Antigravity/antigravity-awesome-skills-main/skills_index.json`
```

Also update the Resources section (if it exists) to:

```markdown
## Resources

- **Local Skills**: `.agent/skills/` (project-specific, check first)
- **Skills Catalog**: `.agent/skills-catalog.md`
- **Memory**: `.agent/memory.md` (update after significant work!)
- **Global Skills Reference**: `~/Desktop/Antigravity/antigravity-awesome-skills-main/skills/`
```

### Step 5: Log the update

Add this entry to `.agent/memory.md` under Session Log:

```markdown
| 2026-02-09 | Applied setup update: local-first skills architecture. Agent now creates project-specific skills in `.agent/skills/` instead of relying on global library. |
```

### Step 6: Verify

```bash
# Confirm create-skill exists
ls .agent/skills/create-skill/SKILL.md

# Confirm skills-catalog was updated
head -3 .agent/skills-catalog.md
```

---

## Done

The project now uses local-first skills. As you work, create skills for repeating patterns using:
```
Read and follow: .agent/skills/create-skill/SKILL.md
```
