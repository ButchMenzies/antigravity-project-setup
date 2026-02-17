---
description: Define your lead generation strategy — channels, lead magnets, outreach, and scaling plan
---

# Lead Strategy

Define how you'll get your offer in front of the right people. Creates a `leads.md` file that guides all lead generation decisions.

## Use this workflow when

- Starting a new product or business that needs leads
- An existing lead pipeline isn't generating enough volume
- You want to formalise which channels you're using and why
- Expanding from one channel to multiple
- Existing marketing "isn't working" and you need a systematic approach

## Pre-flight

1. Read `.agent/AGENT.md` for project context
2. **Find the output location:**
   - Scan the project structure for existing brand/strategy/marketing folders (e.g., `brand/`, `marketing/`, `strategy/`, `docs/brand/`)
   - Use the same folder as `offer.md` if it exists
   - If no relevant folder exists → create `strategy/leads.md` at project root
   - Confirm the location with the user before writing
3. Check if a `leads.md` already exists in the detected location:
   - If it does: show summary and ask what to update
   - If not: proceed with full setup
4. Read `skills/lead-strategy/SKILL.md` — apply lead gen principles throughout
5. If `offer.md` exists, read it — the offer the leads flow into
6. If `.agent/ux/persona.md` exists, read it — these are the people we need to reach

---

## Phase 1: Situation Assessment (always runs)

**CRITICAL RULES:**
- Ask **ONE question per turn**
- Wait for user response before proceeding
- If the user has an existing lead gen setup, import rather than re-asking
- Maximum 7 questions for the full strategy

### Q1: Where are you now?

```
What's your current lead generation situation?

1. Starting from zero — no leads, no audience, no marketing
2. Some traction — I have [describe: some customers, small following, etc.]
3. Working channels — I know what works but need to scale
4. Multiple channels — I need to get organised and optimise
```

### Q2: What's the offer?

```
What are people ultimately buying? (This is what leads flow into.)

1. I've run /offer-strategy — check offer.md
2. Let me describe the offer
3. I don't have a clear offer yet — let's define the basics

[If no clear offer, suggest running /offer-strategy first.
A lead pipeline without a clear offer is a leaky bucket.]
```

### Q3: Where does your audience spend time?

```
Where does your ideal buyer hang out? (This determines channel selection.)

Check all that apply:
1. LinkedIn
2. Instagram
3. YouTube
4. Twitter/X
5. Facebook groups
6. Email (they check business email regularly)
7. Industry events / communities
8. Other: [describe]

I don't know — help me figure it out
```

---

## Phase 2: Channel Strategy

### Q4: Core Four Selection

```
Based on your situation, here's my recommended starting point:

[Based on Q1 answers, suggest 1-2 channels from the Core Four:]

Starting from zero → Warm outreach
Some traction → Warm outreach + Content
Working channels → More of what works (More Better New)
Multiple channels → Audit and optimise

Recommended for you:
- Primary channel: [Channel] — because [reason]
- Secondary channel: [Channel] — because [reason]

1. This looks right — let's plan these channels
2. I'd prefer different channels: [specify]
3. I want to do all four — [suggest starting with fewer]
```

### Q5: Volume Commitment

```
The Rule of 100: 100 primary actions per day for 100 days.

For your chosen channels:

[Channel 1]: Do you commit to [100 messages / 100 minutes / $100] per day?
[Channel 2]: What volume can you sustain?

1. Yes — I can do that
2. That's too much — what's realistic for [time/budget constraints]?
3. Let's figure out a realistic plan
```

---

## Phase 3: Lead Magnets (recommended, opt-in)

```
Do you want to create a lead magnet now?

A lead magnet bridges the gap between "stranger" and "someone who's interested."
It gives them something valuable free in exchange for their contact info.

1. Yes — let's design one
2. I already have one — let me describe it
3. Later — I'll run /lead-strategy again
4. Skip — not relevant for my business
```

If yes or option 2:

### Q6: Lead Magnet Design

```
Let's design your lead magnet.

What narrow problem can you solve for free that naturally leads to your core offer?

The best lead magnets:
- Solve ONE specific problem (not everything)
- Can be consumed in under 10 minutes
- Deliver a quick win or "aha moment"
- Create a gap that your core offer fills

1. I have an idea: [describe]
2. Help me brainstorm — here's my core offer and target audience
```

After gathering the idea, present a draft:

```
Lead Magnet Draft:

Name: [Compelling name]
Format: [PDF / Video / Template / Quiz / etc.]
Problem it solves: [One narrow problem]
Quick win it delivers: [What they can do after consuming it]
Connection to core offer: [How solving this problem reveals the next one]
CTA: [Next step toward core offer]

Does this feel right, or should we adjust?
```

---

## Phase 4: Outreach Planning (if applicable)

If warm or cold outreach is selected:

### Q7: Outreach Approach

```
Let's plan your outreach.

[For warm outreach:]
Who are the first 50 people you could reach out to?
(Past clients, colleagues, friends in the industry, social media connections)

[For cold outreach:]
Who is the ideal person to reach out to?
(Role, company size, industry, specific problem they have)

And what value can you lead with? (Not a pitch — what insight, resource, or help can you offer?)

1. Let me describe my approach
2. Help me craft an outreach message
```

---

## Create Leads File

After gathering all answers, write a single `leads.md` to the detected output location.

The file should contain these sections in order:

```markdown
# Lead Strategy

## Current Situation
[Where they are now with lead generation]

## The Offer (summary)
[What leads flow into — reference offer.md if it exists]

## Channels

### Primary: [Channel Name]
- Why: [reason]
- Volume target: [daily actions]
- Tactics: [specific approach]

### Secondary: [Channel Name]
- Why: [reason]
- Volume target: [daily actions]
- Tactics: [specific approach]

### Future Channels (when ready)
[Channels to consider after mastering primary/secondary]

## Lead Magnet
- Name: [name]
- Format: [format]
- Problem it solves: [problem]
- Quick win: [outcome]
- Connection to core offer: [how it leads to the paid offer]
- CTA: [next step]

## Outreach
### Script/Template
[The outreach message or approach]

### Follow-up Sequence
[Follow-up plan]

### Target List Criteria
[Who to target and how to find them]

## Volume Commitment
[Rule of 100 commitment and tracking plan]

## Scaling Plan
[When and how to apply More Better New]

## Next Steps
[Actionable items based on what was created]
```

---

## Completion

```
✅ Lead strategy created!

File: [path/to/leads.md]

Summary:
- Primary channel: [channel] at [volume]/day
- Lead magnet: [name]
- Outreach: [approach summary]

## What to do next

1. **Start today** — send your first [X] outreach messages or create your first piece of content
2. **Create your lead magnet** — if designed but not yet built
3. **Set up tracking** — simple daily log of actions, responses, and leads generated
4. **Run /offer-strategy** — if you haven't refined the offer these leads flow into
5. **Build a landing page** — the copywriting skill will use leads.md and offer.md to write it
6. **Commit to 100 days** — mark day 1 on the calendar and don't break the chain

→ /offer-strategy — refine the offer these leads flow into
→ /lead-strategy — run again to expand or optimise
```

---

## Re-run Behaviour

When a `leads.md` already exists:

```
I found an existing lead strategy:

Location: [path]
Primary channel: [summary]
Lead magnet: [summary or "Not yet defined"]

What would you like to do?

1. Update a specific section
2. Re-do the full lead strategy
3. Add lead magnets or outreach — if not done yet
4. Scale up — apply More Better New to what's working
5. Review everything — let's read through and refine
```

---

## Guidelines

- **Import over interrogate** — if the user has existing marketing in place, import and adapt.
- **Suggest, don't prescribe** — always offer AI-suggested options AND a "type your own" escape hatch.
- **Start small, then scale** — recommend one primary channel, not four at once.
- **Volume before tactics** — if they're not doing enough, that's the fix. Not a new channel.
- **Smart folder placement** — always scan the project first for existing brand/strategy folders before creating new ones. Use the same location as offer.md if it exists.
- **This is a living system** — the skill will prompt updates as the strategy evolves.
