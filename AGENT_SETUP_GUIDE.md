# 🚀 Antigravity Agent Setup

> **Paste this into a new project chat to bootstrap Antigravity.**
> Self-contained — works on any device, no local files needed.
> Installs slash command workflows, the skill creator, and a minimal AGENT.md.
> Then run `/setup` for interactive project onboarding.

---

## Step 1: Detect Existing Setup

```bash
ls -la .agent/ 2>/dev/null
ls -la .agent/workflows/ 2>/dev/null
```

### If `.agent/workflows/` exists with 8 workflow files — **FULLY SET UP**

This project already has the slash command system. Skip setup entirely.
Read `.agent/AGENT.md` and `.agent/memory.md`, then proceed with the user's request.

### If `.agent/AGENT.md` exists but `.agent/workflows/` is missing or empty — **NEEDS UPGRADE**

1. **Skip to Step 2** to create the directories
2. **Run Steps 3-5** to install workflows, skill, and templates
3. **Add Core Rules and Available Commands** to the existing AGENT.md (see template in Step 5)
4. Tell the user: *"Your project has been upgraded to use slash commands. Run `/setup` to fill in any gaps, or `/status` to see where things stand."*

### If no `.agent/` directory — **FRESH PROJECT**

Proceed with full bootstrap (Steps 2-6).

---

## Step 2: Create Directory Structure

```bash
mkdir -p .agent/workflows
mkdir -p .agent/skills/create-skill
```

---

## Step 3: Install Workflows

Read each file from the GitHub repo and create it locally. The repo is the source of truth.

**Repo:** `https://github.com/ButchMenzies/antigravity-project-setup`
**Raw base:** `https://raw.githubusercontent.com/ButchMenzies/antigravity-project-setup/main`

Read each of these URLs and write the content to the corresponding local path:

| Remote Path | Local Path |
|------------|------------|
| `.agent/workflows/setup.md` | `.agent/workflows/setup.md` |
| `.agent/workflows/new-track.md` | `.agent/workflows/new-track.md` |
| `.agent/workflows/edit.md` | `.agent/workflows/edit.md` |
| `.agent/workflows/implement.md` | `.agent/workflows/implement.md` |
| `.agent/workflows/status.md` | `.agent/workflows/status.md` |
| `.agent/workflows/update-memory.md` | `.agent/workflows/update-memory.md` |
| `.agent/workflows/end-session.md` | `.agent/workflows/end-session.md` |
| `.agent/workflows/create-skill.md` | `.agent/workflows/create-skill.md` |

For each file: read `{raw base}/{remote path}` → write content to local `{local path}`.

---

## Step 4: Install Create-Skill

Read and create locally:

| Remote Path | Local Path |
|------------|------------|
| `.agent/skills/create-skill/SKILL.md` | `.agent/skills/create-skill/SKILL.md` |

---

## Step 5: Install Templates

Read each template from the repo and create locally:

| Remote Path | Local Path |
|------------|------------|
| `templates/AGENT.md` | `.agent/AGENT.md` |
| `templates/memory.md` | `.agent/memory.md` |
| `templates/skills-catalog.md` | `.agent/skills-catalog.md` |

---

## Step 6: Done!

Tell the user:

```
✅ Antigravity bootstrapped!

Installed:
- 8 slash command workflows (/setup, /new-track, /edit, /implement, /status, /update-memory, /end-session, /create-skill)
- Create-skill starter skill
- Minimal AGENT.md with core rules

Next step:
→ Run /setup for interactive project onboarding
```
