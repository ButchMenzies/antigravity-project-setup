---
name: create-skill
description: Create a new project-local skill when you encounter repeating patterns, 
  complex workflows, or project-specific conventions worth codifying.
---

# Create Skill

Create a reusable, project-local skill and save it to `.agent/skills/`.

## Use this skill when

- You've done the same multi-step process **more than once** in this project
- A complex workflow needs documenting for future sessions (e.g., deployment, data migration)
- The project has specific conventions that should be followed consistently
- You solved a hard problem and want to capture the approach

## Do not use this skill when

- The task is a one-off that won't repeat
- A local skill already exists — **update it instead**
- The knowledge is better suited for `memory.md` (a decision or lesson, not a reusable process)

## Instructions

### Step 1: Check for Existing Skills

Before creating a new skill, check if one already exists:

```bash
# Check local skills first
ls .agent/skills/ 2>/dev/null

# Then check global library for inspiration
curl -s https://raw.githubusercontent.com/sickn33/antigravity-awesome-skills/main/skills_index.json | grep -i "<KEYWORD>" | head -10
```

- **Local skill exists?** → Update it instead of creating a duplicate
- **Global skill exists?** → Read its `SKILL.md` for inspiration, then write a local version tailored to this project's conventions

### Step 2: Create the Skill

```bash
mkdir -p .agent/skills/<skill-name>
```

Write `.agent/skills/<skill-name>/SKILL.md` using this template:

```markdown
---
name: <skill-name>
description: <One-line description of what this skill does>
---

# <Skill Title>

<Brief description of what this skill accomplishes.>

## Use this skill when

- <Trigger 1>
- <Trigger 2>

## Do not use this skill when

- <Anti-trigger — when NOT to use this>

## Instructions

### Step 1: <First Step>

<Detailed instructions with exact commands, code patterns, or checklists.>

### Step 2: <Next Step>

<Continue with clear, actionable steps.>

## Examples

<Optional: Include a concrete example of the skill in action.>

## References

<Optional: Links to docs, related skills, or source material.>
```

### Step 3: Register the Skill

Add the new skill to `.agent/skills-catalog.md` under the **Local Skills** section:

```markdown
| `<skill-name>` | <One-line description> |
```

### Step 4: Verify

- [ ] Skill has clear trigger conditions (when to use / when not to use)
- [ ] Instructions are specific enough to follow without guessing
- [ ] Registered in `skills-catalog.md`

## Guidelines

- **Be specific, not generic.** A local skill should capture *this project's* way of doing things, not generic best practices. Include exact file paths, naming conventions, and patterns from the codebase.
- **Keep it actionable.** Every step should be something the agent can execute. Avoid vague advice like "follow best practices."
- **Include commands.** If the skill involves terminal work, include the exact commands.
- **One skill, one job.** A skill should do one thing well. If it's getting long, split it.
- **Update, don't duplicate.** If an existing skill covers 80% of what you need, update it rather than creating a new one.
