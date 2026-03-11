---
version: 1
description: Design and run tests for recent changes or full app QA — scope-adaptive, structured, collaborative
---

# Test

Design, execute, and report on tests. Adapts from quick focused tests (5-10 cases for a recent feature) to comprehensive QA sessions (50+ cases across the whole app) based on what you're testing.

## Rules (Non-Negotiable)

1. **During Phase 4 (Execute): observe and record only. Do NOT fix anything.** Not a bug, not a typo, not a missing import. Record it, move on. Fixing mid-test breaks the systematic view and introduces variables.
2. **Read the code before designing tests.** Every test case must be grounded in what the code actually does, not what you assume it does.
3. **Classify each test honestly.** If you can't reliably test something yourself, say so. Don't fake a pass.

---

## Phase 1: Scope

Brainstorm with the user to understand what's being tested.

**Ask:** "What do you want to test?"

Based on the answer, determine:

1. **What** — a specific feature, a recent track, or the whole app?
2. **Where** — is it part of the active track? An older feature? Something unrelated to current work?
3. **Context** — does a spec/plan exist for this feature? (Check `conductor/tracks/`, `.agent/current-plan.md`)

**Read the relevant context:**
- If a spec exists → read it (gold mine of expected behaviour)
- If no spec → derive expected behaviour from the code and the brainstorm conversation
- Read the actual code files that implement the feature
- Read relevant DB schema, API routes, config if applicable

The depth of this conversation naturally determines the test scope — a quick answer produces a focused test, an expansive answer produces comprehensive QA.

**Do not proceed until the user confirms the scope.**

---

## Phase 2: Reconnaissance

Build ground truth before designing tests. Read the code to establish:

1. **What's built** — which parts of the feature are actually implemented?
2. **What's half-wired** — code exists but isn't connected (dead handlers, unused components, stub functions)?
3. **What's missing** — expected functionality that doesn't exist yet?
4. **Dependencies** — what does this feature depend on? (APIs, DB tables, other components, external services)

For **focused tests**, this is a quick code read of the relevant files. For **full QA**, this is a deeper audit across the architecture — routes, components, API endpoints, DB schema.

Present a brief pre-test summary to the user:

```
📋 Pre-Test Findings

Built and wired:
- [list of what exists and appears functional]

Partially built:
- [anything half-wired or incomplete]

Not implemented:
- [expected features that don't exist yet]

Dependencies:
- [key systems this relies on]
```

---

## Phase 3: Test Design

Design structured test cases based on the scope and reconnaissance.

### Test case format

Each test must have:
- **ID** — sequential (e.g. 1.1, 1.2, 2.1)
- **Action** — what to do (specific, reproducible)
- **Expected** — what should happen (derived from spec, code, or brainstorm)
- **Method** — how to test:
  - 🤖 **Agent** — terminal, API calls, DB queries, code analysis
  - 🌐 **Browser** — agent uses browser tool to interact with UI
  - 👤 **User** — needs human judgement, device access, or real credentials
  - 🤝 **Collaborative** — agent runs the test, user observes and reports what they see

### Ordering

Group tests into **rounds** ordered by dependency:
- Round 1: foundation (does it load/exist?)
- Round 2: core functionality (does the main thing work?)
- Round 3: edge cases and error handling
- Round 4: integration points (does it play well with other features?)

Early failures may make later tests irrelevant — note skipped tests and why.

**Present the test plan to the user for review before executing.** They may add, remove, or reprioritise tests.

---

## Phase 4: Execute

> ⛔ **Observe and record only. Do NOT fix anything during this phase.**

Run tests in order. For each test, record:

| Field | Values |
|-------|--------|
| **Result** | ✅ Pass · ❌ Fail · ⚠️ Partial · ⏭️ Skipped |
| **Notes** | What actually happened, error messages, unexpected behaviour |
| **Root cause** | If fail/partial — what the code is doing wrong (with file references) |

### Execution by method

- **🤖 Agent tests:** Run commands, query APIs/DB, read logs. Record output.
- **🌐 Browser tests:** Navigate, interact, capture what you see. Be honest about what you can and can't verify visually.
- **👤 User tests:** Tell the user exactly what to do. They report the result. Agent captures any relevant logs alongside.
- **🤝 Collaborative:** Agent drives, user watches and reports what they observe.

If a test reveals something unexpected, note it but **do not investigate further during this phase**. Add it to the retest list.

---

## Phase 5: Compile

Compile all results into a structured report.

### Scorecard

| Round | Area | Pass | Partial | Fail | Skipped |
|-------|------|------|---------|------|---------|

### Categorised findings

**🔴 Critical** — crashes, data loss, broken core flows. Include: test ID, impact description, root cause, file references.

**🟠 Major** — features that don't work as expected. Same format.

**🟡 Minor** — cosmetic issues, rough edges, stale text.

**📋 Missing features** — things that should exist but don't. Include priority assessment.

**💡 UX observations** — things that work but feel wrong or could be better.

**🔄 Retest list** — items that need deeper investigation with reason.

### Summary

End with a prioritised list of the top fixes, ordered by impact.

**Present the report to the user.** This is the deliverable of `/test`.

---

## Phase 6: Act

After the user has reviewed the report, ask:

```
What would you like to do with these findings?

1. Update the roadmap with all findings
2. Just add the critical/major bugs to the roadmap
3. Let's fix [specific item] right now
4. Nothing for now — I'll handle this later
```

**Wait for user response.** Only modify living documents (roadmap, product.md, etc.) with explicit approval. The user controls what gets actioned and when.
