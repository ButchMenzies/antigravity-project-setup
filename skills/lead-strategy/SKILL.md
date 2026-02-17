---
name: lead-strategy
description: Frameworks for generating and converting leads using the Core Four channels, lead magnets, and scaling methods. Reads the project's leads.md for project-specific lead gen artifacts.
---

# Lead Strategy

Get your offer in front of the right people. A great offer without leads is a secret. This skill ensures every lead generation decision is intentional, scalable, and connected to the offer.

## Use this skill when

- Building lead magnets, opt-in pages, or free content strategies
- Planning outreach sequences (warm or cold)
- Creating content calendars or social media strategies
- Setting up paid advertising campaigns
- Evaluating lead quality or conversion rates
- Deciding where to focus marketing effort

## Do not use this skill when

- Building or refining the offer itself (use `offer-strategy` instead)
- Writing copy (use `copywriting` — though it should reference the lead strategy)
- Working on product delivery or fulfilment

## Instructions

### Step 1: Read the Lead Strategy Foundation

Before any lead gen work, find and read the project's `leads.md` file (if it exists):

- Check common locations: `strategy/`, `brand/`, `marketing/`, `docs/`, or project root
- The file contains: channels, lead magnets, outreach scripts, volume targets, scaling plan

If no `leads.md` exists, suggest running `/lead-strategy` first.

Also look for:
- `offer.md` — the offer the leads flow into (same locations)
- Persona files — check `brand/`, `.agent/ux/`, or similar — who we're trying to reach

### Step 2: Core Four Assessment

Every business generates leads through four channels. Assess each:

| Channel | Status | Volume | Quality | Next Action |
|---------|--------|--------|---------|-------------|
| **Warm Outreach** | ☐ Active / ☐ Not started | ___/day | High/Med/Low | |
| **Cold Outreach** | ☐ Active / ☐ Not started | ___/day | High/Med/Low | |
| **Content Creation** | ☐ Active / ☐ Not started | ___/week | High/Med/Low | |
| **Paid Ads** | ☐ Active / ☐ Not started | $___/day | High/Med/Low | |

**Key questions:**
- [ ] Which channel is generating the most leads right now?
- [ ] Which channel has the most untapped potential?
- [ ] Are we doing enough volume? (Rule of 100: 100 actions/day)
- [ ] Are channels compounding? (Leads from one channel feeding another)

### Step 3: Lead Magnet Evaluation

If a lead magnet exists or is being created, evaluate it:

- [ ] **Solves a narrow problem** — Not "everything about marketing" but "how to write your first cold email"
- [ ] **Related to core offer** — Solving the lead magnet problem naturally leads to wanting the core offer
- [ ] **Valuable enough to sell** — If it's not good enough to charge for, it's not good enough to give away
- [ ] **Easy to consume** — Can be consumed in under 10 minutes (PDF, short video, template, tool)
- [ ] **Has a clear CTA** — After consuming, the next step toward the core offer is obvious
- [ ] **Named compellingly** — The name conveys the benefit, not the format

### Step 4: Channel-Specific Checklists

#### Warm Outreach
- [ ] Contact list identified (friends, family, past clients, followers, connections)
- [ ] Outreach message is personal, not mass-broadcast
- [ ] Provides value before asking (lead with help, not a pitch)
- [ ] Follow-up sequence defined (not just one message)
- [ ] Ask for referrals — "Who else do you know who might benefit?"
- [ ] Volume target: ___/day

#### Cold Outreach
- [ ] Target list defined (who are we reaching out to?)
- [ ] Research process defined (personalise each message)
- [ ] First message provides value or insight (not a pitch)
- [ ] Follow-up sequence: 3-7 touchpoints over 2-4 weeks
- [ ] Message tested and refined based on response rates
- [ ] Volume target: ___/day
- [ ] Making cold feel warm (reference their work, shared connection, specific insight)

#### Content Creation
- [ ] Platform(s) chosen based on where the persona spends time
- [ ] Content pillars defined (3-5 topics you consistently create about)
- [ ] Content provides genuine value (teach, don't tease)
- [ ] CTA in every piece (follow, subscribe, get lead magnet, etc.)
- [ ] Publishing cadence: ___/week
- [ ] "Give until they ask" — provide enough free value that people seek out the paid offer

#### Paid Ads
- [ ] Target audience defined (demographics, interests, lookalikes)
- [ ] Ad creative tested (images, video, copy variations)
- [ ] Landing page optimised for conversion
- [ ] Lead magnet or offer clearly presented
- [ ] Budget and ROAS targets defined
- [ ] Retargeting set up for warm audiences
- [ ] Daily spend target: $___/day

### Step 5: Volume & Consistency Check

Apply the Rule of 100:

```
Am I doing 100 [primary actions] per day?

- 100 warm outreach messages, OR
- 100 cold outreach messages, OR
- 100 minutes of content creation, OR
- $100/day in paid ads

For 100 consecutive days.
```

If you're not hitting these numbers, volume is the problem before strategy is.

### Step 6: Scaling Assessment

When a channel is working, apply "More Better New":

1. **More** — Do more of what's already working. Same channel, higher volume.
2. **Better** — Improve what's working. Better copy, better targeting, better follow-up.
3. **New** — Only after maxing "more" and "better" — try a new channel or approach.

**Common mistake:** Jumping to "new" before fully exploiting "more" and "better".

### Step 7: Post-Build Update

After completing lead gen work, check if this revealed anything new:

```
Lead Strategy Update:

- [ ] New channel activated → update leads.md (channels section)
- [ ] Lead magnet created/refined → update leads.md (lead magnet section)
- [ ] Outreach sequence created → update leads.md (outreach section)
- [ ] No updates needed
```

## Anti-Patterns

- **The shiny channel** — Jumping to a new platform every month instead of mastering one. Pick one, do it for 100 days, then evaluate.
- **The invisible lead magnet** — Creating a lead magnet but not promoting it. A lead magnet nobody sees generates zero leads.
- **The pitch-first outreach** — Leading with "buy my thing" instead of providing value. Warm or cold, lead with help.
- **The content spray** — Posting everywhere with no strategy. Better to dominate one platform than to be invisible on five.
- **The volume excuse** — "I tried cold email and it didn't work" after sending 10 emails. Volume matters. Rule of 100.
- **The quality-only trap** — Obsessing over perfect content/ads instead of shipping. Done beats perfect. Refine after you have data.

## References

- `resources/core-four.md` — detailed breakdown of each channel
- `resources/lead-magnets.md` — lead magnet creation framework
- `resources/scaling.md` — Rule of 100 and More Better New
- `resources/examples.md` — annotated lead gen examples
- Project `leads.md` — project-specific lead strategy (location varies by project)
- `/lead-strategy` workflow — run to create or update the lead strategy
- `offer-strategy` skill — the offer that leads flow into
- `copywriting` skill — for writing outreach and content copy
