---
description: Define your product's design direction — personas, brand voice, emotional design, visual identity
---

# UX Design

Establish the design foundation for a product. Creates a knowledge base in `.agent/ux/` that guides every UI decision.

## Use this workflow when

- Setting up a new product (offered during `/setup`)
- Starting a redesign or pivot
- The product has no design direction documented
- You want to refine or expand the existing design foundation

## Pre-flight

1. Read `.agent/AGENT.md` for project context
2. Check if `.agent/ux/` already exists:
   - If it does: show summary of existing files and ask what to update
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
Who is the primary user of this product?

I'm looking for the *person*, not the demographic. Tell me about:
- Who they are (age, situation, context)
- What they're feeling when they open this product
- What they tried before

1. I have existing personas — let me paste/point you to them
2. Let me describe the user
3. Let's build one together — ask me questions
```

If option 1: Import the content into `.agent/ux/persona.md`, then confirm and refine with follow-up if needed.
If option 2 or 3: build the persona through conversation.

### Q2: What's the transformation?

```
Every great product promises a transformation:

From: [where the user is now]
  To: [where they want to be]

What's yours? For example:
- From "overwhelmed by options" → To "I have a clear plan I believe in"
- From "I don't know if it's working" → To "I can see my progress"

1. [Suggest based on persona]
2. Type your own
```

### Q3: How should it feel?

```
If this product was a physical space, what would it be?

1. A workshop — equipped, capable, everything in its place
2. A coach's office — calm, encouraging, personal
3. A cockpit — focused, information-rich, in control
4. A café — warm, relaxed, inviting
5. A gym — energizing, motivating, action-oriented
6. Something else: [describe]
```

Use the answer to derive the emotional pillar.

### Q4: Three adjectives

```
Pick 3 words that describe how the product should feel:

Suggested based on your space metaphor:
1. [Adjective 1], [Adjective 2], [Adjective 3]
2. Type your own three
```

### Q5: Brand voice

```
How should this product "talk" to the user?

1. Like a coach — direct, encouraging, clear
2. Like a friend — casual, warm, relatable
3. Like a mentor — knowledgeable, patient, wise
4. Like a professional — precise, confident, efficient
5. Something else: [describe]

Any existing brand guidelines I should follow?
1. Yes — let me share them
2. No — let's define it now
```

If they have brand guidelines: import and adapt.

### Create Foundation Files

After gathering answers, create:

```bash
mkdir -p .agent/ux/journeys .agent/ux/assets
```

Write these files using the templates:
- `.agent/ux/persona.md` — from Q1 answers
- `.agent/ux/design-direction.md` — from Q2, Q3, Q4 answers
- `.agent/ux/brand.md` — from Q5 answers

---

## Phase 2: Visual Identity (recommended, opt-in)

```
Do you want to establish the visual identity now?

This covers colors, typography, and how they connect to the emotional direction we just set.

1. Yes — let's do it
2. Later — I'll run /ux-design again when ready
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
- `.agent/ux/colors.md` — from Q6
- `.agent/ux/typography.md` — from Q7

---

## Phase 3: Reference Files (seed with templates)

These files start thin and grow during development:

- `.agent/ux/patterns.md` — write the template header with empty sections
- `.agent/ux/competitive.md` — write the template header
- `.agent/ux/journeys/README.md` — explain the folder's purpose

---

## Completion

```
✅ Design foundation created!

Files:
- .agent/ux/persona.md — [persona name/summary]
- .agent/ux/brand.md — [voice summary]
- .agent/ux/design-direction.md — [pillar + transformation]
[If Phase 2 completed:]
- .agent/ux/colors.md — [palette summary]
- .agent/ux/typography.md — [font choices]
[Always:]
- .agent/ux/patterns.md — ready for patterns
- .agent/ux/competitive.md — ready for references
- .agent/ux/journeys/ — ready for journey maps

The ux-design skill will reference these files during all UI work.

→ Continue working
→ /new-track — start building with this foundation
→ /ux-design — run again to refine or expand
```

---

## Re-run Behavior

When `.agent/ux/` already exists:

```
I found an existing design foundation:

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
- **Connect to emotion** — every choice should tie back to how the product should *feel*, not just how it should *look*.
- **Keep templates thin** — Phase 3 files should be lightweight starters, not empty 50-line templates that feel like homework.
- **This is a living system** — the skill will prompt updates as the product evolves. The workflow creates the starting point, not the final version.
