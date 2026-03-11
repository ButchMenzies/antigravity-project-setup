---
version: 1
description: Deep audit of conductor documents against the actual codebase — find drift, reconcile vision
---

# Project Audit

Systematically compare conductor documents (`product.md`, `roadmap.md`, `tech-stack.md`) against the actual codebase. Find discrepancies, reconcile vision with reality, and update documents to match what's actually been built.

> **Run this every few sessions** to catch drift that incremental `/end-session` updates miss.

---

## Step 1: Read All Conductor Documents

Read and internalise every conductor document:

1. `conductor/product.md` — note Vision items and Current State/Features
2. `conductor/roadmap.md` — note phase statuses and incomplete items
3. `conductor/tech-stack.md` or `conductor/strategy.md` — note declared stack and decisions
4. `conductor/tracks/*/spec.md` and `plan.md` — note acceptance criteria and task statuses

If any conductor document is missing, note it for the report.

Also read `.agent/AGENT.md` for declared project context.

---

## Step 2: Scan the Actual Codebase

Investigate the real state of the project. Adapt your approach to the project type, but cover these areas:

### For code projects:

**File structure:**
```bash
find . -type f -not -path '*/node_modules/*' -not -path '*/.git/*' -not -path '*/.next/*' -not -path '*/dist/*' | head -100
```

**Dependencies:**
```bash
cat package.json 2>/dev/null | head -50
cat requirements.txt 2>/dev/null
cat go.mod 2>/dev/null
```

**Database schema** (if applicable):
- Check for migration files, Prisma schema, Supabase types, or ORM models
- Use MCP tools if a database connection is available

**Routes and API endpoints:**
- Scan for route definitions, page directories, API handlers

**Key modules:**
- Read main entry points and core logic files
- Identify major features by scanning component/module directories

**Config and infrastructure:**
- Environment variables, deployment configs, auth setup

### For workspace/non-code projects:

- List all documents and their structure
- Check output/deliverable directories
- Review strategy and research files

---

## Step 3: Compare and Flag Discrepancies

Go through each conductor document and check every claim against reality:

### Product.md audit:
- **Features listed but not found** — features in Current State/Features that don't exist in the codebase
- **Features built but not documented** — code that implements functionality not mentioned in product.md
- **Stale descriptions** — features that exist but work differently than documented
- **Vision items already built** — Vision section items that exist in the codebase

### Roadmap.md audit:
- **Completed items still unchecked** — tasks marked incomplete that are actually done
- **Stale phases** — phases with items that are no longer relevant
- **Missing work** — significant work done that isn't reflected in any phase
- **Ordering issues** — phases that should be reordered based on current state

### Tech-stack.md audit:
- **Dependencies mismatch** — packages in package.json not in tech-stack.md, or vice versa
- **Infrastructure changes** — services added or removed that aren't reflected
- **Stale decisions** — decisions that were reversed or superseded

---

## Step 4: Vision Reconciliation

Review each item in the Vision section of `product.md`:

For each vision item, categorise it:

1. **Built** — this feature exists in the codebase → propose moving to Current State with an accurate description
2. **In progress** — partially built or on the active roadmap → keep in Vision
3. **Still planned** — not built, but still on the roadmap → keep in Vision
4. **No longer relevant** — not built, not on the roadmap, overtaken by events → propose removal

Present findings to the user:

```
🔍 Vision Reconciliation

Built (move to Current State):
- [Item] — found in [file/module], works as [description]

Still planned:
- [Item] — on roadmap Phase [N]

Propose removal:
- [Item] — not on roadmap, no code found. Still relevant?
```

**Wait for user response** before modifying Vision items.

---

## Step 5: Present Audit Report

Summarise all findings in a structured report:

```
📊 Project Audit Report

## Product.md
- Features documented: [N]
- Features found in code: [N]
- Undocumented features: [list]
- Stale entries: [list]
- Vision items to reconcile: [N]

## Roadmap.md
- Phases: [completed]/[total]
- Items needing status update: [list]
- Stale items: [list]

## Tech Stack
- Dependencies in sync: [yes/partial/no]
- Mismatches: [list]

## Recommended Updates
1. [Specific update with rationale]
2. [Specific update with rationale]
...
```

---

## Step 6: Apply Approved Changes

For each recommended update, apply it to the relevant conductor document:

1. **Product.md updates** — move vision items, add undocumented features, fix stale descriptions
2. **Roadmap.md updates** — mark completed items, remove stale phases, reorder if needed
3. **Tech-stack.md updates** — sync dependencies, update decisions

Show the user what was changed after applying:

```
✅ Audit complete

Updated:
- conductor/product.md — [summary of changes]
- conductor/roadmap.md — [summary of changes]
- conductor/tech-stack.md — [summary of changes]

Next audit recommended after [N] more sessions of active development.
```
