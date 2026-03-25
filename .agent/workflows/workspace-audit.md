---
name: workspace-audit
version: 1
description: >-
  Performs a deep audit of workspace documents against actual content
  to find drift and reconcile vision. Use when workspace documentation
  may be outdated or inconsistent.
source: antigravity
---

# Workspace Audit

Examine the workspace to check whether conductor documents (the vision) match reality (what actually exists). Flag drift, stale references, and untracked work.

## Step 1: Read the Vision

Read all conductor documents for the stated vision:

| Document | What to extract |
|----------|----------------|
| `conductor/product.md` | What the workspace claims to contain. Current state, goals |
| `conductor/roadmap.md` | Phases, what's marked done vs. in-progress vs. planned |
| `conductor/strategy.md` | Goals, audience, approach, growth priorities |

If any of these don't exist, note it — that's itself a finding.

## Step 2: Scan the Workspace

List all directories and their contents. Focus on:

| Area | What to check |
|------|--------------|
| `output/` | Content files — articles, scripts, posts, documents |
| `research/` | Research notes, analysis, references |
| `brand/` | Brand identity files — persona, voice, visual identity |
| `assets/` | Images, graphics, media files |
| `conductor/tracks/` | Completed and in-progress tracks |
| Root level | Any files that should be organised into folders |

## Step 3: Compare and Flag

Check for these categories of drift:

### 📋 Roadmap Drift
- Items marked as ✅ that don't have corresponding output
- Items still marked 🔄 that are actually complete
- Phases that are stale or no longer relevant

### 📄 Content Drift
- Content in `output/` not reflected in product.md
- Strategy goals that aren't being acted on
- Documents referenced elsewhere but never created

### 📁 Organisation Issues
- Orphaned files with no clear purpose
- Files in wrong directories
- Missing directories that should exist based on strategy

### 🎯 Strategy Alignment
- Work being done that doesn't align with stated goals
- Goals that have drifted from the original vision
- Gaps between what's planned and what's needed

## Step 4: Vision Reconciliation

For each significant finding, determine:
1. **Is the vision wrong?** (Strategy says X, but we've learned Y is better)
2. **Is the reality wrong?** (Strategy says X, we just haven't done it yet)
3. **Is it drift?** (Nobody decided to change, it just... drifted)

This distinction matters for the recommendations.

## Step 5: Present Report

```
# Workspace Audit Report

## Health Summary
- Vision documents: [all present / missing: X]
- Content output: [N pieces across M categories]
- Strategy alignment: [strong / moderate / weak]

## Findings

### 🔴 Critical (blocks progress)
- [Finding 1]

### 🟡 Important (should address soon)
- [Finding 2]

### 🟢 Minor (nice to fix)
- [Finding 3]

## Recommended Actions
1. [Action — update product.md to reflect X]
2. [Action — create missing Y]
3. [Action — reorganise Z]
```

**Wait for user response.**

## Step 6: Apply Approved Changes

For each approved action:
1. Make the change
2. Verify it's consistent with other documents
3. Note the change in memory.md

Do not make changes the user hasn't approved.
