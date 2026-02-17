---
name: offer-strategy
description: Frameworks for building irresistible offers using the Value Equation and Grand Slam Offer methodology. Reads the project's offer.md for project-specific offer artifacts.
---

# Offer Strategy

Build offers so good people feel stupid saying no. Every offer decision should maximize perceived value while minimizing perceived cost to the buyer.

## Use this skill when

- Building or refining a product offer, pricing page, or sales page
- Creating bonus stacks, guarantees, or value propositions
- Evaluating whether an existing offer is compelling enough
- Writing offer-related copy (use alongside `copywriting` skill)
- Launching a new product or service
- A prospect says "I need to think about it" too often

## Do not use this skill when

- Working on lead generation (use `lead-strategy` instead)
- Writing general content that isn't offer-specific
- The product doesn't have a defined target market yet (define the market first)

## Instructions

### Step 1: Read the Offer Foundation

Before any offer work, find and read the project's `offer.md` file (if it exists):

- Check common locations: `strategy/`, `brand/`, `marketing/`, `docs/`, or project root
- The file contains: market definition, value stack, bonuses, guarantee, scarcity, naming

If no `offer.md` exists, suggest running `/offer-strategy` first.

Also look for persona files (check `brand/`, `.agent/ux/`, or similar) — the offer must speak to the persona's pain.

### Step 2: Value Equation Check

Every offer decision maps to the Value Equation:

```
Value = (Dream Outcome × Perceived Likelihood) / (Time Delay × Effort & Sacrifice)
```

For any offer element, ask:

- [ ] **Dream Outcome** — Is the desired end result crystal clear? Can the buyer picture it?
- [ ] **Perceived Likelihood** — Does the buyer believe they will actually achieve it? (Proof, testimonials, case studies, credentials)
- [ ] **Time Delay** — How fast do they get results? Can we reduce time to first value?
- [ ] **Effort & Sacrifice** — What do they have to give up? Can we reduce friction?

**The goal**: Maximize the top (outcome × likelihood), minimize the bottom (time × effort).

### Step 3: Offer Completeness Audit

A Grand Slam Offer has all of these. Check each:

| Element | Status | Notes |
|---------|--------|-------|
| **Dream Outcome** defined | ☐ | What transformation does the buyer get? |
| **All problems listed** | ☐ | Every obstacle between buyer and outcome |
| **Solution for each problem** | ☐ | Not just the main problem — every sub-problem |
| **Solutions trimmed & stacked** | ☐ | High-value, low-cost solutions kept. Stacked for perceived value |
| **Bonuses** | ☐ | Additional value that eclipses the core offer |
| **Guarantee** | ☐ | Risk reversal — what do they get if it doesn't work? |
| **Scarcity** | ☐ | Limited quantity or availability (real, not manufactured) |
| **Urgency** | ☐ | Time-based reason to act now |
| **Naming** | ☐ | Compelling name that conveys value |
| **Premium pricing** | ☐ | Priced for value, not cost. Higher price = higher perceived value |

### Step 4: Enhancer Review

For each enhancer, verify it's genuine and well-executed:

**Bonuses:**
- Does each bonus solve a related problem?
- Is the bonus valuable enough to sell on its own?
- Does the total bonus value exceed the core offer price?

**Guarantees:**
- What type? (Unconditional, conditional, anti-guarantee, implied)
- Does it remove the buyer's biggest objection?
- Is it specific? ("30-day money-back" beats "satisfaction guaranteed")

**Scarcity:**
- Is it real? (Manufactured scarcity destroys trust)
- Is it clearly communicated?

**Urgency:**
- Is there a legitimate deadline?
- What happens after the deadline? (Bonus removed, price increase, enrollment closes)

**Naming:**
- Does the name convey the transformation, not just the mechanism?
- Does it stand alone without explanation?

### Step 5: Price Positioning

Check pricing against these principles:

- [ ] Price reflects value delivered, not hours worked or cost of goods
- [ ] Price is high enough to fund excellent delivery and client success
- [ ] Price anchoring is used (show the value stack total vs. the actual price)
- [ ] Price creates a "no-brainer" gap between value and cost

### Step 6: Post-Build Update

After completing offer work, check if this revealed anything new:

```
Offer Knowledge Update:

- [ ] Offer structure changed → update offer.md (value stack section)
- [ ] New market insight → update offer.md (buyer/market section)
- [ ] Enhancers refined → update offer.md (enhancers section)
- [ ] No updates needed
```

## Anti-Patterns

- **The feature list** — Listing what's included instead of what changes for the buyer. Nobody buys features. They buy outcomes.
- **The race to the bottom** — Competing on price instead of value. If the only differentiator is price, the offer needs work.
- **The fake guarantee** — "100% satisfaction guaranteed" means nothing. Be specific about what happens if it doesn't work.
- **The kitchen sink** — Stacking everything you can think of instead of trimming to high-value items. More isn't better. More *valuable* is better.
- **The invisible offer** — A great product with no articulated offer. If you can't describe the value stack in 60 seconds, it's not an offer yet.
- **Selling the mechanism** — "8-week coaching program" describes the mechanism. "Go from confused to confident with a clear 90-day plan" describes the transformation.

## References

- `resources/value-equation.md` — the Value Equation framework in detail
- `resources/offer-creation.md` — Grand Slam Offer step-by-step process
- `resources/enhancers.md` — bonuses, guarantees, scarcity, urgency, naming
- `resources/examples.md` — annotated before/after offer examples
- Project `offer.md` — project-specific offer strategy (location varies by project)
- `/offer-strategy` workflow — run to create or update the offer foundation
- `lead-strategy` skill — for lead generation (offers attract, leads fill the pipeline)
- `copywriting` skill — for writing the offer copy
