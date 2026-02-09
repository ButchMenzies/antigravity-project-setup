---
description: Create a new project-local skill from repeating patterns or workflows
---

# Create Skill

Create a reusable, project-local skill. Use when you notice repeating patterns, complex workflows, or project conventions worth codifying.

## Steps

### 1. Check for Existing Skills

Before creating anything, check if the skill already exists:

**Step 1a: Check local skills**
```bash
ls .agent/skills/ 2>/dev/null
```
If a local skill already exists for this pattern → **update it instead**.

**Step 1b: Check the Antigravity skills library (your curated skills)**

Read the skills index from the setup repo:
```
https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/skills/README.md
```

If the skill exists there → **copy it directly** into `.agent/skills/<skill-name>/` and customize for the current project if needed.

**Step 1c: Check the global reference library (for inspiration)**

Only if the skill doesn't exist in the Antigravity library:
```
https://raw.githubusercontent.com/sickn33/antigravity-awesome-skills/main/skills_index.json
```
Search for relevant keywords. If a global skill matches → read its SKILL.md for inspiration, then write a local version tailored to this project.

### 2. Gather Details

```
Skill name (kebab-case, e.g., "api-error-handling"):
One-line description:
When should this skill be used?
When should it NOT be used?
```

### 3. Create the Skill

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

### 4. Register the Skill

Add to `.agent/skills-catalog.md` under Local Skills:

```markdown
| `<skill-name>` | <One-line description> |
```

### 5. Upload to Antigravity Skills Library (if owner)

**Only attempt this if the user has write access to the repo.**

First, check:
```bash
gh auth status 2>&1 | head -5
```

- If `gh` is not installed or not authenticated → **skip this step silently**
- If authenticated, check if the user can push to `ButchMenzies/antigravity-project-setup`:
  ```bash
  gh repo view ButchMenzies/antigravity-project-setup --json viewerPermission --jq '.viewerPermission' 2>/dev/null
  ```
- If permission is `ADMIN` or `WRITE` → proceed with upload
- Otherwise → **skip this step silently**

**To upload:** Clone the repo to a temp directory, add the skill, commit, push, clean up:
```bash
git clone https://github.com/ButchMenzies/antigravity-project-setup.git /tmp/antigravity-setup
cp -r .agent/skills/<skill-name> /tmp/antigravity-setup/skills/<skill-name>
# Update skills/README.md with the new skill entry
cd /tmp/antigravity-setup && git add -A && git commit -m "Add skill: <skill-name>" && git push
rm -rf /tmp/antigravity-setup
```

### 6. Confirm

```
✅ Skill created: <skill-name>

Location: .agent/skills/<skill-name>/SKILL.md
Registered in: .agent/skills-catalog.md
[If uploaded]: Uploaded to: antigravity-project-setup/skills/<skill-name>/
[If not uploaded]: Saved locally (no write access to skills library)

→ Continue working
→ /status — see project overview
```

## Guidelines

- **Be specific, not generic.** Capture THIS project's way of doing things.
- **Keep it actionable.** Every step should be executable without guessing.
- **Include commands.** If it involves terminal work, include exact commands.
- **One skill, one job.** A skill should do one thing well. If it's getting long, split it.
- **Update, don't duplicate.** If an existing skill covers 80% of what you need, update it.
- **Upload general skills.** If the skill would be useful in other projects, upload it to the Antigravity repo.
