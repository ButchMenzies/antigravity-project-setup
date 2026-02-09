---
description: Create a new project-local skill from repeating patterns or workflows
---

# Create Skill

Create a reusable, project-local skill. Use when you notice repeating patterns, complex workflows, or project conventions worth codifying.

## Steps

### 1. Identify the Pattern

```
What pattern or workflow should this skill capture?

1. A multi-step process I keep repeating
2. A project-specific convention or pattern
3. A workflow that needs documenting
4. Something inspired by a global skill I want to customize
```

### 2. Check for Existing Skills

```bash
# Check local skills
ls .agent/skills/ 2>/dev/null

# Search global library for inspiration
curl -s https://raw.githubusercontent.com/sickn33/antigravity-awesome-skills/main/skills_index.json | grep -i "<keyword>" | head -10
```

- If a local skill already exists → suggest updating it instead
- If a global skill matches → read it for inspiration, then write a tailored local version

### 3. Gather Details

```
Skill name (kebab-case, e.g., "api-error-handling"):
One-line description:
When should this skill be used?
When should it NOT be used?
```

### 4. Create the Skill

```bash
mkdir -p .agent/skills/<skill-name>
```

Write `.agent/skills/<skill-name>/SKILL.md`:

```markdown
---
name: <skill-name>
description: <One-line description>
---

# <Skill Title>

<Brief description>

## Use this skill when

- <Trigger 1>
- <Trigger 2>

## Do not use this skill when

- <Anti-trigger>

## Instructions

### Step 1: <First Step>

<Detailed, actionable instructions with exact commands and code patterns>

### Step 2: <Next Step>

<Continue with clear steps>
```

### 5. Register the Skill

Add to `.agent/skills-catalog.md` under Local Skills:

```markdown
| `<skill-name>` | <One-line description> |
```

### 6. Confirm

```
✅ Skill created: <skill-name>

Location: .agent/skills/<skill-name>/SKILL.md
Registered in: .agent/skills-catalog.md

The skill will be available in future sessions.

→ Continue working
→ /status — see project overview
```

## Guidelines

- **Be specific, not generic.** Capture THIS project's way of doing things.
- **Keep it actionable.** Every step should be executable without guessing.
- **Include commands.** If it involves terminal work, include exact commands.
- **One skill, one job.** If it's getting long, split it.
- **Update, don't duplicate.** If an existing skill covers 80% of what you need, update it.
