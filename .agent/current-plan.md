# Skill Bundles per Project Type

## Critical Analysis

### What We're Solving

The agent builds projects without domain-specific guidance. It knows how to write code but repeatedly makes the same operational mistakes: forgetting RLS policies, duplicating components, skipping type regeneration, mixing server/client boundaries. There's no mechanism to inject project-type-specific expertise into the planning and implementation flow.

### What We Agreed

1. **Shared skill pool + bundle manifests** — skills are written once and reused across project types via lightweight bundle files that list which skills to install
2. **Invocation at `/new-track`** — the agent scans the installed skills at planning time, picks the relevant ones, and bakes their checklists into the implementation plan
3. **MCP-aware skills** — three tiers: pure workflow, MCP-enhanced (graceful fallback), MCP-dependent
4. **Component reuse enforcement** — a cross-cutting concern embedded in Build Component and Create Feature skills
5. **~15-18 shared skills total**, not 85+ duplicated copies

### What's Strong

- **The `/new-track` integration point** is natural and low-friction. The hook already exists (line 14 of `new-track.md`), it just needs strengthening. Skills influence the plan, not the execution — which means they actually get followed.
- **Shared pool architecture** eliminates the maintenance nightmare. One Fix Bug skill serves 5 project types.
- **Bundle manifests are trivial** — adding/removing a skill from a project type is one line change.
- **This fits the existing system** — same pattern as workflows (source of truth in this repo, copied to projects). No new infrastructure needed.

### What Needs Attention

- **Skill format is pivotal.** If skills are long-form documentation, the agent will skim them. If they're structured checklists with clear triggers and plan items, they work. Format must prioritize machine consumption.
- **Starting scope.** Writing 15-18 skills at once is too much. Start with the Web App bundle (most commonly used), prove the pattern, then expand.
- **MCP availability checking.** Skills that reference MCPs need a graceful "if available, use it; otherwise, guide manually" pattern. This should be a convention in the skill format, not ad-hoc per skill.
- **Update mechanism for existing projects.** When a new skill is added to the repo, existing projects don't get it automatically. This is an existing gap with workflows too — not a blocker, but worth noting.

### What's Risky

- **Skill rot.** If skills reference specific library APIs or patterns that change, they go stale silently. Mitigation: keep skills focused on *process and checklists*, not API specifics.
- **Over-reliance on skills.** If the agent reads a skill that says "check for existing components" but the project has no components yet, it wastes time. Skills need to be smart about context.
- **Plan bloat.** If `/new-track` loads 3 skills and each adds 5 checklist items, the plan grows by 15 steps. Some tracks (quick bug fixes) shouldn't get that overhead. Need lightweight matching.

---

## Proposed Changes

### Architecture

```
skills/
├── shared/                        # The actual skill files (~15-18)
│   ├── create-feature/SKILL.md
│   ├── fix-bug/SKILL.md
│   ├── database-change/SKILL.md
│   ├── build-component/SKILL.md
│   ├── build-page/SKILL.md
│   ├── create-endpoint/SKILL.md
│   ├── performance-audit/SKILL.md
│   ├── auth-flow/SKILL.md
│   ├── payments-stripe/SKILL.md
│   ├── seo-audit/SKILL.md
│   ├── api-design/SKILL.md
│   ├── deploy-vercel/SKILL.md
│   ├── deploy-railway/SKILL.md
│   ├── package-publish/SKILL.md
│   └── ...
└── bundles/
    ├── web-app.md
    ├── website.md
    ├── api-backend.md
    ├── mobile-app.md
    ├── cli-tool.md
    ├── library.md
    └── workspace.md
```

### Skill Format

Each `SKILL.md` should be structured for plan integration, not long-form reading:

```markdown
---
name: Create Feature
description: Full vertical slice — database to UI
triggers: [new feature, new functionality, user-facing change]
track_types: [feature]
mcps:
  - name: supabase
    usage: migration, RLS check, type generation
    required: false
---

# Create Feature

## When to Use
When the track involves building new user-facing functionality that touches
multiple layers (database, API, UI).

## Plan Checklist
Items to incorporate into the implementation plan phases:

### Pre-build
- [ ] Scan `components/` for existing components that can be reused or extended
- [ ] Identify database schema changes needed

### Database (if applicable)
- [ ] Write and apply migration
- [ ] Add/verify RLS policies for new tables/columns
- [ ] Regenerate TypeScript types from schema
- [ ] MCP: Run Supabase security advisor check

### API Layer
- [ ] Create server actions or API routes
- [ ] Add input validation
- [ ] Add error handling with typed responses

### UI Layer
- [ ] Build with existing shared components where possible
- [ ] Include loading state (loading.tsx or Suspense)
- [ ] Include error state (error.tsx or error boundary)
- [ ] Include empty state
- [ ] Verify responsive at mobile/tablet/desktop
- [ ] Check against UX design artifacts if they exist

### Verification
- [ ] Feature works end-to-end
- [ ] RLS policies tested (correct user can access, others cannot)
- [ ] Types compile with no errors

## Common Mistakes
- Building UI before confirming the data model
- Forgetting loading/error/empty states
- Creating new components when similar ones already exist
- Skipping RLS policies on new Supabase tables
- Not regenerating TypeScript types after schema changes
```

### Bundle Manifest Format

Each bundle is a simple list:

```markdown
# Web App Bundle

Skills to install for web application projects (Next.js, React, etc.)

## Skills
- create-feature
- fix-bug
- database-change
- build-component
- performance-audit
- auth-flow
- deploy-vercel
- payments-stripe

## MCPs (Recommended)
- supabase — database, auth, RLS management
- vercel — deployment and preview environments
```

### Workflow Changes

#### `/new-project` (Phase 2)

After installing workflows and existing skills, add:

1. Read the bundle manifest matching the project type from Q1
2. Copy only the listed skills from `shared/` to `.agent/skills/`

#### `/new-track` (Pre-flight, line 14)

Strengthen from "check for relevant skills" to:

> Read `.agent/skills/` — for each skill, check if its `triggers` and `track_types` match the current track. For matching skills, read their **Plan Checklist** and incorporate relevant items into the implementation plan phases.

---

## Implementation Phases

### Phase 1: Foundation (start here)

1. Define the skill format (SKILL.md frontmatter + structured sections as above)
2. Write **3 core shared skills** to prove the pattern:
   - `create-feature` — the most common and complex
   - `fix-bug` — the most universal
   - `database-change` — the most MCP-integrated
3. Write the **web-app bundle manifest**
4. Update `/new-project` Phase 2 to read bundle and copy matching skills
5. Update `/new-track` pre-flight to scan and match skills by trigger/track type
6. Test by scaffolding a new web app project and running `/new-track` for a feature

### Phase 2: Expand Skills

7. Write remaining shared skills: `build-component`, `performance-audit`, `auth-flow`, `deploy-vercel`, `payments-stripe`, `seo-audit`, `api-design`, `create-endpoint`, `build-page`, `deploy-railway`, `package-publish`
8. Write remaining bundle manifests: `website`, `api-backend`, `mobile-app`, `cli-tool`, `library`, `workspace`

### Phase 3: MCP Integration

9. Document MCP availability checking pattern
10. Add MCP-specific steps to relevant skills (Supabase queries, Vercel deploys)
11. Add MCP recommendation section to bundle manifests

---

## Verification Plan

### Phase 1 Verification
1. Scaffold a fresh Next.js web app project using `/new-project`
2. Confirm the 3 core skills were installed to `.agent/skills/`
3. Run `/new-track` for a feature (e.g., "add a comments system")
4. Verify the generated plan includes checklist items from the matched skills (RLS check, type regen, component reuse scan, loading/error states)
5. Run `/new-track` for a bug — verify only Fix Bug skill items appear, not Create Feature items

### Phase 2 Verification
6. Scaffold each project type and verify correct bundle installed
7. Spot check plan generation for each type

---

## Next Steps

After this plan is approved:
→ `/implement` to start with Phase 1
