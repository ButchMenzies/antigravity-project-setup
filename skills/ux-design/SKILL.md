---
name: ux-design
description: >-
  Provides a reference checklist for applying the product's design direction
  during UI implementation. Reads and updates the project's brand and design
  files. Use when implementing UI, reviewing design consistency, or updating
  brand guidelines.
source: antigravity
---

# UX Design

Ensures every UI decision connects to the product's design foundation. Read the project's brand/design files before building, and update them after.

## Use this skill when

- Building any new user-facing screen, page, or component
- Designing a user flow or interaction pattern
- Writing user-facing copy (use alongside `copywriting` skill)
- Reviewing or critiquing existing UI
- A new feature needs a journey map

## Do not use this skill when

- Working on backend/API code only
- Building internal tooling where design direction doesn't apply
- The project has no brand/design files (suggest running `/ux-design` first)

## Instructions

### Step 1: Read the Design Foundation

Before any UI work, find and read the project's brand/design files. Check common locations: `brand/`, `branding/`, `design/`, `.agent/ux/`, or project root.

Look for these files (if they exist):

1. **`persona.md`** — Who am I building for? What are they feeling?
2. **`design-direction.md`** — What emotional pillar guides this product? What's the transformation?
3. **`brand.md`** — How should this product sound and feel?
4. **`colors.md`** — What palette am I using and what does each color mean?
5. **`typography.md`** — What fonts and scale?
6. **`patterns.md`** — Is there an established pattern for this type of UI?
7. **`journeys/`** — Is there a journey map for this feature?

If critical files are missing (persona, design-direction, brand), suggest running `/ux-design` before proceeding.

### Step 2: Pre-Build Checklist

Before writing UI code, answer these questions:

- [ ] **Who**: Does this screen serve the persona's needs and emotional state?
- [ ] **Feel**: Does the design match the emotional pillar? (Check adjectives in design-direction.md)
- [ ] **Transformation**: Does this screen contribute to the user's transformation, or is it just... there?
- [ ] **Voice**: Does the copy match the brand voice?
- [ ] **Differentiation**: Am I using the curated palette and typography, or falling back to defaults?

### Step 3: Differentiation Check

Before finalizing any UI, check against these common "vibe-coded" defaults:

| Element | 🚫 Generic default | ✅ Intentional alternative |
|---|---|---|
| Color | Framework defaults or random Tailwind | Curated palette from `colors.md` |
| Typography | system-ui / Inter (unless deliberately chosen) | Fonts from `typography.md` with rationale |
| Layout | Standard card grid, uniform spacing | Layout that serves the content and journey |
| Motion | None or generic fade-in | Purposeful animation tied to meaning |
| Empty states | "No data yet" + button | Encouraging, showing what's possible |
| Loading | Spinner | Skeleton or contextual loading that maintains layout |
| Copy | Generic labels ("Submit", "Items") | Persona-appropriate language from `brand.md` |

### Step 4: Journey Mapping (for new features)

When building a new feature, map the emotional journey before designing screens:

1. **Arrival** — What emotion does the user bring? (Confused? Curious? Anxious?)
2. **First value** — How quickly do they feel "this is for me"? (Target: < 30 seconds)
3. **Investment** — What makes them put something of themselves into it?
4. **Reward** — What's the "I did it" moment?
5. **Return** — Why do they come back?

Save the journey map to `journeys/[feature-name].md` in the same design folder.

### Step 5: Post-Build Update

After completing UI work, check if this revealed anything new:

```
UX Knowledge Update:

- [ ] New UI pattern established → update patterns.md
- [ ] New journey mapped → create journeys/[feature-name].md
- [ ] Color usage refined → update colors.md
- [ ] Persona insight discovered → update persona.md
- [ ] Brand voice evolved → update brand.md
- [ ] Competitive reference found → update competitive.md
- [ ] Typography usage clarified → update typography.md
- [ ] No updates needed
```

Update the relevant files. This keeps the design knowledge growing with the product.

## Anti-Patterns

- **Skipping the persona read** — building UI without knowing who it's for leads to generic design
- **Treating design-direction.md as decoration** — if the pillar says "calm confidence" but the screen is cluttered and loud, the file isn't doing its job
- **Empty patterns.md** — after 3+ screens, this file should have entries. If it's still empty, you're not capturing patterns
- **Ignoring the differentiation check** — one quick scan catches 80% of "looks like every other app" issues
- **Copy-pasting UI patterns** — reusing component code is fine, but every screen should be checked against the persona and journey

## References

- Project brand/design files — the product's design knowledge base (location varies by project)
- `/ux-design` workflow — run to create or update the design foundation
- `copywriting` skill — for tone and grammar rules
- `tailwind-design-tokens` skill — for implementation-level design patterns (if using Tailwind)
