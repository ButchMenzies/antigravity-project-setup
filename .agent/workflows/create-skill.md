---
description: Create a new project-local skill from repeating patterns or workflows
---

# Create Skill

Create a reusable, project-local skill and save it to `.agent/skills/`.

## Use this workflow when

- You've done the same multi-step process **more than once** in this project
- A complex workflow needs documenting for future sessions (e.g., deployment, data migration)
- The project has specific conventions that should be followed consistently
- You solved a hard problem and want to capture the approach
- The user wants to create a new domain-specific capability (e.g., "write marketing copy")

## Do not use this workflow when

- The task is a one-off that won't repeat
- A local skill already exists — **update it instead**
- The knowledge is better suited for `memory.md` (a decision or lesson, not a reusable process)

---

## Step 1: Discover Existing Skills

Before creating anything, search broadly — the skill may already exist under a different name.

**1a. Check local skills:**
```bash
ls .agent/skills/ 2>/dev/null
```
If a local skill covers this → **update it** instead of creating a duplicate.

**1b. Check the Antigravity skills library (curated, tested skills):**

Read: `https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main/skills/README.md`

**Search with 3-5 related keywords, not just the exact name.** Think about what the skill is *about*, not just what the user *called* it.

Examples:
- User says "copywriting skill" → search: `copywriting`, `marketing`, `copy`, `content`, `writing`
- User says "deployment skill" → search: `deployment`, `deploy`, `release`, `CI`, `pipeline`
- User says "component skill" → search: `component`, `UI`, `design-system`, `layout`, `pattern`

**1c. Check the global reference library:**
```bash
curl -s https://raw.githubusercontent.com/sickn33/antigravity-awesome-skills/main/skills_index.json | grep -iE "<KEYWORD1>|<KEYWORD2>|<KEYWORD3>" | head -10
```

**If matches are found**, present them with context:

```
I found some existing skills that might cover what you need:

| Skill | Description | Match |
|-------|-------------|-------|
| `<name>` | <description> | Strong — covers this directly |
| `<name>` | <description> | Partial — related but different focus |

1. Install one and customize it for this project
2. Use one as a starting point but rewrite significantly
3. These aren't what I need — create from scratch
```

**Wait for user response before proceeding.**

If installing an existing skill → copy it to `.agent/skills/<skill-name>/`, customize for the project, register in catalog. **Done — skip remaining steps.**

---

## Step 2: Choose Depth

```
What kind of skill is this?

1. Quick — Simple process, I just need it documented
   (e.g., deployment steps, git conventions, file naming rules)

2. Solid — Important workflow that needs to work well
   (e.g., component patterns, API design, testing approach)

3. Deep — High-impact skill I'll reuse across projects
   (e.g., marketing copy, code review, design system, architecture)
```

**Wait for user response before proceeding.**

---

## Step 3: Scope

Ask targeted questions based on the chosen depth.

### Quick (1-2 questions)
Confirm what the skill does and any edge cases:
```
Quick check before I document this:
- What exactly should this skill cover?
- Anything it should NOT do?
```

**Wait for user response, then skip to Step 5.**

### Solid (2-3 questions)
Understand scope, standards, and context:
```
A few questions to get this right:
1. What exactly should this skill do? (the core job)
2. Are there existing patterns in this project I should follow?
3. Any specific quality standards or conventions to enforce?
```

**Wait for user response before proceeding.**

### Deep (3-5 questions)
Understand the domain, audience, influences, and quality bar:
```
To build a really strong skill, I need to understand the domain:

1. What's the core job this skill needs to do?
2. Who is the target audience / end user for the output?
3. What does "great" look like? Any examples, brands, or people whose work you admire?
4. Are there specific frameworks, methodologies, or principles to follow?
5. Any anti-patterns — things you've seen done badly that this should avoid?
```

**Wait for user response before proceeding.**

---

## Step 4: Research (Solid and Deep only)

> Skip this step for **Quick** skills.

### Solid — Light Research

1. **Check existing codebase patterns** — scan the project for related files, conventions, or prior implementations
2. **Web search** — 1-2 searches for best practices and established frameworks:
   ```
   search_web("<skill domain> best practices <framework>")
   ```
3. **Read 1-2 top sources** — use `read_url_content` on the most relevant results

Synthesize findings into a mental model. Move to Step 4b.

### Deep — Full Research

1. **Check existing codebase patterns** — same as Solid
2. **Web search** — 2-3 searches covering the domain from different angles:
   ```
   search_web("<skill domain> best practices principles")
   search_web("<named expert/brand> approach to <domain>")
   search_web("<domain> frameworks methodologies comparison")
   ```
3. **Read 3-5 top sources** — use `read_url_content` on the best results, especially any references the user named in Step 3
4. **Study named influences** — if the user mentioned specific people, brands, or styles, research them thoroughly. Extract their core principles, signature patterns, and what makes their approach distinctive.

Compile research into notes for the `resources/` folder (created in Step 5).

### Step 4b: Draft Outline (Solid and Deep)

Present a **draft outline** of the skill before writing it:

```
Here's what I'm thinking for the <skill-name> skill:

## Structure
1. <Step 1 title> — <what it covers>
2. <Step 2 title> — <what it covers>
3. <Step 3 title> — <what it covers>

## Key Principles (from research)
- <Principle 1>
- <Principle 2>
- <Principle 3>

## Examples I'll Include
- <Example 1 description>
- <Example 2 description>

Does this cover what you need, or should I adjust?
```

**Wait for user response before proceeding.**

---

## Step 5: Create the Skill

```bash
mkdir -p .agent/skills/<skill-name>
```

Write `.agent/skills/<skill-name>/SKILL.md` using the template below.

**For Deep skills only** — also create the resources folder:
```bash
mkdir -p .agent/skills/<skill-name>/resources
```

And write research artifacts:
- `.agent/skills/<skill-name>/resources/frameworks.md` — methodologies, principles, and approaches discovered during research
- `.agent/skills/<skill-name>/resources/references.md` — notes on named influences, what makes their approach distinctive, key takeaways
- `.agent/skills/<skill-name>/resources/examples.md` — concrete before/after examples, annotated with why the good version works

### Skill Template

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

<Required for Deep skills. Encouraged for Solid. Optional for Quick.>
<Include concrete examples of the skill in action — not hypothetical, real.>
<For Deep skills: annotate examples with WHY they work.>

## References

<Optional: Links to docs, related skills, or source material.>
<For Deep skills: reference the resources/ folder for detailed notes.>
```

### Quality bar by tier

| | Quick | Solid | Deep |
|---|---|---|---|
| **Instructions** | Concise steps | Detailed with rationale | Comprehensive with principles |
| **Examples** | Optional | Encouraged (1-2) | Required (2-3, annotated) |
| **Resources folder** | No | No | Yes |
| **Anti-patterns** | Optional | Encouraged | Required — what to avoid and why |

---

## Step 6: Test (Solid and Deep only)

> Skip this step for **Quick** skills.

Do a quick dry run — generate one sample output using the skill you just created. Show it to the user:

```
Here's a sample of what this skill would produce:

---
<sample output>
---

Does this match the quality you're after? I can adjust the skill if needed.
```

**Wait for user response.** If adjustments needed → update the skill, then continue.

---

## Step 7: Register

Add the new skill to `.agent/skills-catalog.md` under the **Local Skills** section:

```markdown
| `<skill-name>` | <One-line description> |
```

---

## Step 8: Upload to Antigravity Skills Library (if owner)

Check `which gh` → if not installed, skip entirely. Then `gh auth status`:
- Not authenticated → queue to `.agent/pending-skill-uploads.md` (silent, no user message)
- Authenticated → check `gh repo view ButchMenzies/antigravity-project-setup --json viewerPermission`
  - `ADMIN`/`WRITE` → upload skill + drain any pending queue, then delete the queue file
  - Other → skip silently

**Before uploading a new skill, also check for pending uploads:**

If `.agent/pending-skill-uploads.md` exists and we have write access, upload all pending skills too, then delete the file.

**To upload:**
```bash
git clone https://github.com/ButchMenzies/antigravity-project-setup.git /tmp/antigravity-setup
cp -r .agent/skills/<skill-name> /tmp/antigravity-setup/skills/<skill-name>
# Also copy any other pending skills
# Update skills/README.md with new entries
cd /tmp/antigravity-setup && git add -A && git commit -m "Add skill: <skill-name>" && git push
rm -rf /tmp/antigravity-setup
```

After successful upload, delete `.agent/pending-skill-uploads.md` if it existed.

---

## Step 9: Confirm

```
✅ Skill created: <skill-name>

Location: .agent/skills/<skill-name>/SKILL.md
Registered in: .agent/skills-catalog.md
[If uploaded]: Uploaded to: antigravity-project-setup/skills/<skill-name>/
[If queued]: Queued for upload (will upload when authenticated)

→ Continue working
→ /status — see project overview
```

---

## Guidelines

- **Be specific, not generic.** A local skill should capture *this project's* way of doing things, not generic best practices. Include exact file paths, naming conventions, and patterns from the codebase.
- **Keep it actionable.** Every step should be something the agent can execute. Avoid vague advice like "follow best practices."
- **Include commands.** If the skill involves terminal work, include the exact commands.
- **One skill, one job.** A skill should do one thing well. If it's getting long, split it.
- **Update, don't duplicate.** If an existing skill covers 80% of what you need, update it rather than creating a new one.
- **Upload general skills.** If the skill would be useful in other projects, upload it to the Antigravity repo.
- **Skills are living documents.** After using a skill 2-3 times, review it for refinements. Patterns that worked well should be reinforced. Patterns that needed manual adjustment should be updated. Suggest a refinement pass to the user when you notice this.
