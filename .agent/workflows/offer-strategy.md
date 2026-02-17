---
description: Build a Grand Slam Offer — define dream outcome, stack value, add enhancers, create irresistible pricing
---

# Offer Strategy

Build a compelling offer using the Value Equation and Grand Slam Offer methodology. Creates an `offer.md` file that guides pricing, sales copy, and product decisions.

## Use this workflow when

- Launching a new product or service
- An existing offer isn't converting well enough
- Pricing feels wrong (too low, too high, or competing on price)
- You need to articulate why someone should buy
- Starting a new project that will sell something

## Pre-flight

1. Read `.agent/AGENT.md` for project context
2. **Find the output location:**
   - Scan the project structure for existing brand/strategy/marketing folders (e.g., `brand/`, `marketing/`, `strategy/`, `docs/brand/`)
   - If a relevant folder exists → use it (e.g., `brand/offers/offer.md`)
   - If not → create `strategy/offer.md` at project root
   - Confirm the location with the user before writing
3. Check if an `offer.md` already exists in the detected location:
   - If it does: show summary and ask what to update
   - If not: proceed with full setup
4. Read `skills/offer-strategy/SKILL.md` — apply offer principles throughout
5. If `.agent/ux/persona.md` exists, read it — the offer must speak to this person

---

## Phase 1: Market & Dream Outcome (always runs)

**CRITICAL RULES:**
- Ask **ONE question per turn**
- Wait for user response before proceeding
- If the user has existing offers or market research, import rather than re-asking
- Maximum 7 questions for the full offer

### Q1: Who is this for?

```
Who is the ideal buyer for this offer?

I'm looking for the *person with the problem*, not just a demographic:
- What's their current situation?
- What pain or frustration brought them here?
- What have they tried before?

1. I already have a persona defined (check .agent/ux/persona.md)
2. Let me describe them
3. Let's figure it out together
```

If option 1: Read the persona and confirm it applies to this offer.

### Q2: What's the dream outcome?

```
What does the buyer actually want to achieve?

Not your product. Not your service. The END STATE they're paying for.

Template:
From: [where they are now — specific pain]
  To: [where they want to be — specific gain]

For example:
- From "posting on social media and hoping" → To "10 qualified leads per week from social media"
- From "spreadsheets and guesswork" → To "real-time dashboard showing exactly what's working"

1. [Suggest based on persona/context]
2. Type your own
```

### Q3: What are the problems?

```
What obstacles stand between the buyer and that dream outcome?

Think about every step of their journey:
- Before they start: What confuses them? What scares them?
- During the process: Where do they get stuck? What takes too long?
- After they succeed: Can they sustain it? What's next?

List as many as you can think of — we'll prioritise later.

1. Let me list them
2. Help me brainstorm — ask me questions
```

---

## Phase 2: Solution Stack

### Q4: What solutions do you offer?

```
For each problem, what solution do you (or could you) provide?

Consider delivery types:
- Done-for-you (DFY) — you do the work (highest value)
- Done-with-you (DWY) — you guide them (high value)
- Do-it-yourself (DIY) — you give tools/knowledge (medium value)

1. I have existing solutions — let me describe them
2. Help me map solutions to the problems we listed
3. I'm not sure yet — let's brainstorm
```

After gathering solutions, trim and stack:
- Keep high-value, low-cost solutions
- Remove filler
- Stack for maximum perceived value

Present the draft value stack:

```
Here's your proposed value stack:

| Solution | Delivery | Value |
|----------|----------|-------|
| [Solution 1] | [DFY/DWY/DIY] | $[Value] |
| [Solution 2] | [DFY/DWY/DIY] | $[Value] |
| [Solution 3] | [DFY/DWY/DIY] | $[Value] |

Total perceived value: $[Sum]

Add, remove, or adjust?
```

---

## Phase 3: Enhancers (recommended, opt-in)

```
Do you want to add offer enhancers now?

This covers bonuses, guarantees, scarcity, urgency, and naming.

1. Yes — let's make this irresistible
2. Later — I'll run /offer-strategy again when ready
3. Skip — I'll handle enhancers myself
```

If yes:

### Q5: Bonuses

```
What additional value could you include that costs you little but solves a real problem for the buyer?

Good bonuses:
- Tools, templates, or checklists they'd pay for
- Access to community or experts
- A quick-win that gets them started fast
- Training on a related topic

1. I have ideas — let me list them
2. Suggest bonuses based on the problem list
```

### Q6: Guarantee & Risk Reversal

```
What guarantee can you offer?

Options:
1. Unconditional — "30-day money back, no questions asked"
2. Conditional — "If you [do X] and don't get [result], full refund"
3. Performance — "If we don't deliver [result], we keep working for free"
4. Anti-guarantee — "All sales final" (for premium, commitment-based offers)
5. I'm not sure — help me choose

What's the buyer's biggest fear about purchasing?
```

### Q7: Scarcity, Urgency & Naming

```
Final enhancers:

Scarcity — Is there a real limit?
1. Limited spots (I can only serve X at a time)
2. Cohort-based (enrollment opens/closes)
3. No natural scarcity — skip this
4. Other: [describe]

Urgency — Is there a real deadline?
1. Launch pricing (increases after date)
2. Bonus removal (bonuses disappear after date)
3. Seasonal (tied to a calendar event)
4. No natural urgency — skip this

Naming — What should we call this offer?
1. [Suggest 2-3 names based on transformation]
2. I have a name in mind
3. Help me brainstorm
```

---

## Create Offer File

After gathering all answers, write a single `offer.md` to the detected output location.

The file should contain these sections in order:

```markdown
# [Offer Name]

## The Buyer
[Who they are, their situation, pain, and what they've tried]

## The Dream Outcome
From: [current state]
To: [desired state]

## Problems
### Before they start
### During the process
### After they succeed

## Value Stack
[Table: Solution | Delivery | Perceived Value]
Total perceived value: $[sum]

## Bonuses
[Table: Bonus | What it solves | Value]
Total bonus value: $[sum]

## Total Value vs. Price
[Anchor value vs. actual price]

## Enhancers

### Guarantee
[The guarantee and why it works]

### Scarcity
[Real limits, if any]

### Urgency
[Deadlines, if any]

### Naming
[The offer name and why it works]

## Presentation Order
[How to present the offer: pain → dream → solutions → bonuses → value → guarantee → scarcity]

## Next Steps
[Actionable items based on what was created]
```

---

## Completion

```
✅ Offer strategy created: [Offer Name]

File: [path/to/offer.md]

Summary:
- Buyer: [one-line summary]
- Value: $[perceived total] → $[price or TBD]
- Guarantee: [one-line summary]
- Scarcity: [one-line summary]

## What to do next

1. **Set your price** — perceived value is $[X], pick a price point that feels like a no-brainer
2. **Build a landing page** — the copywriting skill will use offer.md to write persuasive copy
3. **Run /lead-strategy** — define how you'll get this offer in front of the right people
4. **Create a lead magnet** — the entry point that pulls people toward this offer
5. **Write an outreach script** — if warm/cold outreach is your channel

→ /lead-strategy — define your lead generation channels
→ /offer-strategy — run again to refine or expand
```

---

## Re-run Behaviour

When an `offer.md` already exists:

```
I found an existing offer strategy:

Offer: [name]
Location: [path]
Buyer: [summary]
Value: $[total]

What would you like to do?

1. Update a specific section
2. Re-do the full offer strategy
3. Add enhancers — if not done yet
4. Review everything — let's read through and refine
```

---

## Guidelines

- **Import over interrogate** — if the user has existing offers, import and adapt. Don't re-ask what they already know.
- **Suggest, don't prescribe** — always offer AI-suggested options AND a "type your own" escape hatch.
- **Value Equation is the filter** — every decision should map back to increasing dream outcome/likelihood or decreasing time/effort.
- **Smart folder placement** — always scan the project first for existing brand/strategy folders before creating new ones.
- **This is a living system** — the skill will prompt updates as the offer evolves. The workflow creates the starting point, not the final version.
