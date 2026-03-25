---
name: brand-design
version: 1
description: >-
  Defines a brand's design direction including personas, brand voice,
  emotional design, and visual identity. Use when establishing or
  refining brand guidelines, visual identity, or design language.
source: antigravity
---

# Brand Design

Establish the brand foundation for a workspace project. Creates brand and design files that guide every piece of content.

## Use this workflow when

- Setting up a new workspace's brand identity
- Refining or expanding the existing brand foundation
- The workspace has no design direction documented
- Starting a new content direction or pivot

## Pre-flight

1. Read `.agent/AGENT.md` for project context
2. **Find the output location:**
   - Scan the project structure for existing brand/design folders (e.g., `brand/`, `branding/`, `design/`, `docs/brand/`)
   - If a relevant folder exists → use it for brand artifacts (e.g., `brand/persona.md`, `brand/design-direction.md`)
   - If not → create `brand/` at project root
   - Confirm the location with the user before writing
3. Check if brand files already exist in the detected location:
   - If they do: show summary of existing files and ask what to update
   - If not: proceed with full setup

---

## Phase 1: Foundation (always runs)

**CRITICAL RULES:**
- Ask **ONE question per turn**
- Wait for user response before proceeding
- If the user has existing answers (e.g., personas), import them rather than re-asking
- Maximum 7 questions for the full foundation

### Q1: Who is this for?

```
Who is the primary audience for this brand?

I'm looking for the *person*, not the demographic. Tell me about:
- Who they are (age, situation, context)
- What they're feeling when they engage with this content
- What else they follow or consume

1. I have existing personas — let me paste/point you to them
2. Let me describe the audience
3. Let's build one together — ask me questions
```

If option 1: Import the content into `brand/persona.md`, then confirm and refine with follow-up if needed.
If option 2 or 3: build the persona through conversation.

### Q2: What's the transformation?

```
Every great brand promises a transformation:

From: [where the audience is now]
  To: [where they want to be]

What's yours? For example:
- From "confused beginner" → To "confident practitioner"
- From "overwhelmed by options" → To "clear on the right path"

1. [Suggest based on persona]
2. Type your own
```

### Q3: How should it feel?

```
If this brand was a physical space, what would it be?

1. A workshop — hands-on, practical, building things
2. A library — thoughtful, curated, deep knowledge
3. A café — casual, welcoming, good conversation
4. A boardroom — strategic, focused, results-driven
5. A studio — creative, experimental, inspiring
6. Something else: [describe]
```

Use the answer to derive the emotional pillar.

### Q4: Three adjectives

```
Pick 3 words that describe how the brand should feel:

Suggested based on your space metaphor:
1. [Adjective 1], [Adjective 2], [Adjective 3]
2. Type your own three
```

### Q5: Brand voice

```
How should this brand "talk" to the audience?

1. Like a mentor — knowledgeable, encouraging, direct
2. Like a friend — casual, honest, relatable
3. Like an expert — authoritative, precise, research-backed
4. Like a coach — motivating, action-oriented, structured
5. Something else: [describe]

Any existing brand guidelines I should follow?
1. Yes — let me share them
2. No — let's define it now
```

If they have brand guidelines: import and adapt.

### Create Foundation Files

After gathering answers, create the files in the detected output location. Also create `journeys/` subfolder.

Write these files using the templates:
- `persona.md` — from Q1 answers
- `design-direction.md` — from Q2, Q3, Q4 answers
- `brand.md` — from Q5 answers

---

## Phase 2: Visual Identity (recommended, opt-in)

```
Do you want to establish the visual identity now?

This covers colors, typography, and how they connect to the emotional direction we just set.
Useful for creating consistent graphics, thumbnails, and social content.

1. Yes — let's do it
2. Later — I'll run /brand-design again when ready
3. Skip — I'll handle visual identity myself
```

If yes:

### Q6: Color direction

```
Based on your emotional pillar ([pillar name]), here are some palette directions:

1. [Warm palette] — [description of feeling]
2. [Cool palette] — [description of feeling]
3. [Neutral palette] — [description of feeling]
4. I have existing brand colors: [paste hex values]
5. Something else: [describe]
```

### Q7: Typography personality

```
What personality should the typography have?

1. Rounded and approachable (e.g., Nunito, Quicksand)
2. Clean and modern (e.g., Inter, Plus Jakarta Sans)
3. Bold and confident (e.g., Montserrat, Outfit)
4. Elegant and refined (e.g., Playfair Display + Lato)
5. I have existing font choices: [specify]
```

Write:
- `colors.md` — from Q6 (in same output location)
- `typography.md` — from Q7 (in same output location)

---

## Phase 3: Reference Files (seed with templates)

These files start thin and grow over time (created in same output location):

- `patterns.md` — write the template header with empty sections
- `competitive.md` — write the template header
- `journeys/README.md` — explain the folder's purpose

---

## Completion

```
✅ Brand foundation created!

Location: [path/to/brand/ or wherever files were written]

Files:
- persona.md — [persona name/summary]
- brand.md — [voice summary]
- design-direction.md — [pillar + transformation]
[If Phase 2 completed:]
- colors.md — [palette summary]
- typography.md — [font choices]
[Always:]
- patterns.md — ready for patterns
- competitive.md — ready for references
- journeys/ — ready for journey maps

## What to do next

1. **Start creating content** — the brand foundation is set
2. **Run /new-track** — plan your first piece of work using this foundation
3. **Run /offer-strategy** — define an offer this brand delivers
4. **Run /brand-design again** — to add visual identity or refine later
```

---

## Re-run Behavior

When brand files already exist:

```
I found an existing brand foundation:

Location: [path]

| File | Status | Summary |
|------|--------|---------|
| persona.md | ✅ | [first line summary] |
| brand.md | ✅ | [first line summary] |
| design-direction.md | ✅ | [first line summary] |
| colors.md | [✅ or 📝 template] | [summary or "Not yet populated"] |
| typography.md | [✅ or 📝 template] | [summary or "Not yet populated"] |
| patterns.md | [✅ or 📝 template] | [summary or "Not yet populated"] |
| competitive.md | [✅ or 📝 template] | [summary or "Not yet populated"] |

What would you like to do?

1. Update a specific file — [choose which]
2. Re-do the full foundation (keeps assets/)
3. Add Phase 2 (visual identity) — if not done yet
4. Review everything — let's read through and refine
```

---

## Guidelines

- **Import over interrogate** — if the user has existing brand docs, personas, or design systems, import and adapt. Don't re-ask what they already know.
- **Suggest, don't prescribe** — always offer AI-suggested options AND a "type your own" escape hatch.
- **Connect to emotion** — every choice should tie back to how the brand should *feel*, not just how it should *look*.
- **Keep templates thin** — Phase 3 files should be lightweight starters, not empty 50-line templates that feel like homework.
- **This is a living system** — the skill will prompt updates as the brand evolves. The workflow creates the starting point, not the final version.
