---
name: voice-notes-triage
description: Analyse voice transcription notes from testing sessions to extract bugs, tasks, and improvements into an actionable plan.
---

# Voice Notes Triage

Parse raw voice transcription from app testing sessions and turn it into a structured, prioritised list of bug fixes, tasks, and improvements — ready to become a plan.

## Use this skill when

- The user pastes a voice transcription or dictation notes
- The user says they have "testing notes", "voice notes", or "feedback from testing"
- The user provides unstructured stream-of-consciousness notes about the app

## Do not use this skill when

- The user already has a structured list of tasks — just use `/new-track` directly
- The user is giving a single, clear bug report or feature request — handle it normally

## Instructions

### Step 1: Receive the Transcript

Accept the raw transcript from the user. It will typically be:
- Informal, conversational language
- Non-linear (jumping between topics)
- Mixed with filler words, corrections, and thinking-out-loud
- Possibly missing punctuation or with speech-to-text errors

Tell the user you've received it and will analyse it now.

### Step 2: Analyse and Extract Items

Read through the entire transcript carefully. For each distinct observation, classify it into one of these categories:

| Category | Icon | Description |
|----------|------|-------------|
| **Bug** | 🐛 | Something broken, not working as expected, or producing errors |
| **UX Issue** | 🎨 | Works but feels wrong — awkward flow, confusing UI, poor feedback |
| **Missing Feature** | ✨ | Something the user expected to exist but doesn't |
| **Improvement** | 🔧 | Something that works but could be better — performance, polish, copy |
| **Question** | ❓ | Something the user was unsure about — needs investigation or decision |

**Extraction rules:**
- **Deduplicate** — if the same issue is mentioned multiple times, consolidate into one item
- **Infer intent** — voice notes are often vague. Use context and your knowledge of the codebase to make items specific and actionable. Quote the original words for reference
- **Preserve nuance** — if the user expressed urgency or frustration about something, note it
- **Separate compound items** — "the form is broken and also ugly" is two items (a bug and a UX issue)

### Step 3: Prioritise

Assign each item a priority:

| Priority | Meaning |
|----------|---------|
| **P1 — Critical** | Blocks core functionality, data loss risk, or broken user flow |
| **P2 — Important** | Visible issue that degrades the experience but has a workaround |
| **P3 — Nice to have** | Polish, minor improvements, or edge cases |

### Step 4: Present the Triage Report

Present the findings in this format:

```markdown
## 📋 Voice Notes Triage Report

**Source:** [Testing session description, e.g. "Mission Control testing — 11 Feb 2026"]
**Items found:** [N total] — [X bugs, Y UX issues, Z improvements, etc.]

---

### P1 — Critical

#### 🐛 [Short title]
> "[Original quote from transcript]"

**What's happening:** [Clear description of the issue]
**Expected:** [What should happen instead]
**Likely location:** [File or component, if you can infer it]

---

### P2 — Important

#### 🎨 [Short title]
> "[Original quote from transcript]"

**What's happening:** [Description]
**Suggestion:** [How to fix or improve]

---

### P3 — Nice to Have

#### 🔧 [Short title]
> "[Original quote from transcript]"

**Suggestion:** [Description]

---

### ❓ Questions / Needs Investigation

#### [Short title]
> "[Original quote from transcript]"

**What to investigate:** [Description]
```

### Step 5: Offer Next Steps

After presenting the report, ask:

```
What would you like to do?

1. → /new-track — turn selected items into an implementation plan
2. → Pick specific items to work on now
3. → Add more notes (another transcript to merge in)
4. → Save report to .agent/triage-reports/ for later
```

### Step 6: Save if Requested

If the user wants to save the report:

```bash
mkdir -p .agent/triage-reports
```

Save to `.agent/triage-reports/[YYYY-MM-DD]-[short-description].md`

## Anti-Patterns — What to Avoid

- **Don't over-split.** "The button is small and hard to tap" is one UX issue, not two. Only separate items when they genuinely require different fixes in different places.
- **Don't invent problems.** Stick to what the user actually said. If the transcript mentions "the form feels slow", report it as a perceived performance issue — don't assume there's a database bottleneck unless the user said so.
- **Don't lose the user's words.** Always include the original quote. The triage report is most useful when the developer can trace an item back to exactly what the user observed. Paraphrasing without quoting loses nuance and urgency.
- **Don't default everything to P2.** Be honest about priority. If the app launches and a core flow is broken, that's P1. If a font looks slightly off, that's P3. Grouping everything in the middle makes the report useless for planning.
- **Don't ignore context clues.** If the user says "I was on the planning page and...", use that to infer the likely file or component. A triage item with a likely location is far more actionable than one without.
- **Don't skip deduplication.** Voice notes are repetitive — the user will often circle back to the same issue multiple times, sometimes with different wording. Consolidate these into one item with the best description, but mention it was raised multiple times (this signals importance).

## Tips for Better Results

- **Longer transcripts are fine.** The skill is designed to handle messy, long, stream-of-consciousness input. Don't trim your notes before pasting.
- **Context helps.** If you mention which page or feature you're testing, the agent can better locate the relevant code.
- **Multiple sessions can be merged.** Run the skill again with a new transcript and ask to merge — the agent will deduplicate across sessions.

## Example

### Raw Transcript Input

> ok so I'm looking at the planning page and the back button is gone again, I think we fixed that before but it's not there. Also the date picker is really annoying, like you have to click through month by month to get to a date that's like 6 months away, it should let you type or jump ahead. Umm what else... oh yeah when I save a plan it takes forever, like 3-4 seconds maybe, the button just sits there and I don't know if it's working. And the goals section — when I first open it the validation errors are already showing before I've typed anything which is really confusing. One more thing, I wonder if we should add a way to duplicate a plan, like copy an existing one as a starting point.

### Triage Report Output

```markdown
## 📋 Voice Notes Triage Report

**Source:** Planning page testing — 11 Feb 2026
**Items found:** 5 total — 1 bug, 2 UX issues, 1 improvement, 1 missing feature

---

### P1 — Critical

#### 🐛 Back button missing on plan detail page
> "the back button is gone again, I think we fixed that before but it's not there"

**What's happening:** Navigation regression — the back button on the plan detail page has disappeared, possibly due to a recent change.
**Expected:** A visible back button to return to the plans list.
**Likely location:** `client/src/pages/planning/plan-detail.tsx`

---

### P2 — Important

#### 🎨 Date picker requires tedious month-by-month navigation
> "you have to click through month by month to get to a date that's like 6 months away, it should let you type or jump ahead"

**What's happening:** The date picker only supports sequential month navigation, making future dates painful to reach.
**Suggestion:** Switch to a date picker with year/month dropdowns or allow direct keyboard input.

#### 🎨 Premature validation errors in goals section
> "when I first open it the validation errors are already showing before I've typed anything which is really confusing"

**What's happening:** Form validation fires on mount instead of on blur/submit, showing errors before the user has interacted.
**Suggestion:** Defer validation until the user touches a field or submits the form.

---

### P3 — Nice to Have

#### 🔧 Slow plan save with no feedback
> "when I save a plan it takes forever, like 3-4 seconds maybe, the button just sits there"

**Suggestion:** Add a loading spinner or disabled state to the save button, and show a success toast on completion. Also investigate if the save endpoint can be optimised.

#### ✨ Duplicate/copy plan feature
> "I wonder if we should add a way to duplicate a plan, like copy an existing one as a starting point"

**Suggestion:** Add a "Duplicate Plan" action that clones an existing plan's structure and goals into a new draft.
```

## References

- [Chromium Bug Triage Best Practices](https://www.chromium.org/developers/bug-triage/) — P0-P3 prioritisation framework used in the priority levels
- [Smartbear: Bug Triage Process](https://smartbear.com/) — Guidance on assessing bug impact and structuring triage meetings
- [QA Session Note-Taking](https://marker.io/) — Structured approaches to capturing bugs during testing sessions, including shorthand notation (B for bug, ? for question, I for idea)
