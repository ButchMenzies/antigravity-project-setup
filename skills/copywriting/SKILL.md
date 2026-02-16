---
name: copywriting
description: Voice, tone, and writing conventions for all user-facing text. Reads .agent/ux/ to ensure copy matches the product's design direction.
---

# Copywriting

Write user-facing text that sounds human, feels warm, and drives action. Every word should serve the persona and the product's emotional direction.

## Use this skill when

- Writing or editing UI labels, descriptions, headings, or CTAs
- Writing help content, tooltips, onboarding copy, or banners
- Writing error messages, empty states, or confirmation messages
- Reviewing existing copy for voice consistency
- Rewriting copy from a corporate/generic tone to the product voice

## Do not use this skill when

- Writing code comments or internal documentation (those can be informal)
- Writing technical API docs (different audience, different rules)

## Instructions

### Step 1: Read the Design Foundation

Before writing any copy, read these files:

1. **`.agent/ux/persona.md`** — Who are you writing for? What are they feeling?
2. **`.agent/ux/brand.md`** — What voice archetype? What do/don't?
3. **`.agent/ux/design-direction.md`** — What's the transformation? What emotional pillar?

If these files don't exist, suggest running `/ux-design` first.

**Write for one person.** Picture the persona. Write as if you're sitting across from them, explaining something over coffee. This is how you avoid generic marketing speak.

### Step 2: Voice Rules

The voice is defined in `brand.md`. These rules apply universally:

| Principle | Do | Don't |
|-----------|-----|-------|
| **Direct** | Get to the point in the first sentence | Bury the lead in setup or context |
| **Specific** | Use concrete numbers, outcomes, timeframes | Use vague words like "optimize", "leverage", "solutions" |
| **Human** | Write like you talk. Read it aloud. | Write like a press release or LinkedIn post |
| **Value-first** | Lead with what the reader gains | Lead with what you do or who you are |
| **Encouraging** | Celebrate progress, normalize difficulty | Patronize or use empty cheerfulness |
| **Honest** | Say "this takes practice" when it does | Overpromise or make it sound effortless |

### Step 3: Sentence-Level Rules

**Hard rules (never break these):**

- **No em dashes (—)** in user-facing text. Use periods, commas, or colons instead.
  - ❌ "Start with your foundation — it guides everything"
  - ✅ "Start with your foundation. It guides everything."
- **No semicolons** in UI copy. Split into two sentences.
- **No walls of text.** Maximum 3 sentences per paragraph in UI copy. If you need more, use a list.
- **Active voice always.**
  - ❌ "A roadmap can be created"
  - ✅ "Create your roadmap"
- **Sentence case** for headings and button labels (e.g., "Create your roadmap", not "Create Your Roadmap").
  - Exception: proper nouns and product names.

**Strong preferences (break only with good reason):**

- Keep sentences under 20 words.
- One idea per sentence.
- Use "you" and "your", not "users" or "clients".
- Use present tense and imperative mood for actions.
- Avoid adverbs. If you need one, the verb is wrong.

### Step 4: Writing Patterns

#### Headlines
- Lead with the transformation or benefit, not the feature.
- Use the reader's language, not industry jargon.
- ❌ "AI-Powered Business Optimization Solutions"
- ✅ "Get real results from AI. Not just generic outputs."

#### CTAs (Buttons & Links)
- Use inviting, low-pressure language. No hard sells.
- Start with a verb that implies collaboration or progress.
- ❌ "Submit", "Buy Now", "Contact Sales"
- ✅ "Let's talk", "Start here", "Show me how"

#### Descriptions & Body Copy
- Open with the pain or desire the reader already has.
- Follow with what changes (the transformation).
- Close with the next step.
- Keep paragraphs to 2-3 sentences max.

#### Empty States
- Include: icon, clear heading (what can be done), brief explanation (why it matters), CTA button.
- Tone: encouraging, forward-looking. Show what's possible, not what's missing.
- ❌ "No data yet"
- ✅ "Your first AI win starts here. Let's find the right place to begin."

#### Error Messages
- Lead with what happened (don't blame the user).
- Follow with what to do next.
- ❌ "Error: Invalid input"
- ✅ "That didn't quite work. Try a different approach, or reach out and we'll help."

#### Loading & Progress
- Use contextual messages over generic spinners where possible.
- ❌ "Loading..."
- ✅ "Getting your roadmap ready..."

### Step 5: AI-Copy Detector

Before finalizing any copy, run this checklist. If you answer "yes" to any, rewrite:

- [ ] Does it use words like "leverage", "utilize", "streamline", "empower", "harness", "elevate", or "unlock"?
- [ ] Does it sound like it could be on any company's website? (The "logo swap" test)
- [ ] Does it use more than 2 commas in a single sentence?
- [ ] Does it contain an em dash?
- [ ] Would the persona actually say this out loud? If not, simplify.
- [ ] Does it start with "We" instead of "You"?
- [ ] Does it use passive voice?
- [ ] Is there a paragraph longer than 3 sentences?

### Step 6: Vocabulary Guide

| Use | Avoid |
|-----|-------|
| help | empower (ironic given the brand name, but in copy it sounds hollow) |
| show you how | enable you to |
| guide | solution |
| real results | optimize outcomes |
| your business | organizations |
| get started | commence |
| AI tools | AI-powered solutions |
| works for you | seamlessly integrates |
| try | leverage |
| save time | enhance productivity |
| clear plan | strategic framework |

**Note:** The brand name "Empowered Growth" is the one place "empowered" is fine. Don't use it elsewhere in copy.

### Step 7: Post-Write Review

After writing copy, check:

- [ ] Read it aloud. Does it sound like a real person?
- [ ] Does every sentence pass the AI-Copy Detector (Step 5)?
- [ ] Does the tone match the voice archetype in `brand.md`?
- [ ] Is the copy scannable? (Short paragraphs, clear headings, lists where appropriate)
- [ ] Would the persona feel understood, not sold to?

## Anti-Patterns

- **The LinkedIn voice** — "Excited to announce", "I'm humbled", formulaic engagement-bait. Kill it.
- **The wall of text** — If someone has to scroll to find the point, they won't.
- **The empty promise** — "Everything you need" means nothing. Be specific about what they get.
- **The jargon shield** — Using complex words to sound smart. It pushes the persona away.
- **The feature dump** — Listing what the product does instead of what changes for the reader.
- **Corporate we** — "We are committed to delivering..." Nobody talks like this. "We'll show you how" works.

## References

- `.agent/ux/brand.md` — voice archetype and do/don't table
- `.agent/ux/persona.md` — who you're writing for
- `.agent/ux/design-direction.md` — emotional pillar and transformation
- `resources/frameworks.md` — writing frameworks from Hormozi, Dan Koe, Stelzner
- `resources/references.md` — influence analysis and key takeaways
- `resources/examples.md` — annotated before/after rewrites
