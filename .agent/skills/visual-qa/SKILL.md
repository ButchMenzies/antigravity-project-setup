---
name: Visual QA & Design Audit
description: Achieve pixel-perfect implementation by applying captured target data to code and verifying visually.
---

# Visual QA & Design Audit Skill

**Role**: ENGINE — The reusable logic for building Section Briefs, applying data to code, and verifying results. Called by `/recreate-site`, `/compare-site`, or directly.

## Core Principle: Data First, Visual Second

The captured `-data-*.png` files contain exact CSS values. Use these as the primary source of truth. Visual screenshots are for verification, not discovery.

---

## Step 1: Build Section Brief (from captured data)

**Inputs**:
- `[page]-[0n]-[section]-data-container.png`
- `[page]-[0n]-[section]-data-[element].png` (one or more)
- `.agent/design-system.md` (global tokens)

**Process**:
1. Read ALL `-data-*` files for this section.
2. Extract every value into a structured **Section Brief**:

```markdown
## Section: [Name]
### Container
- display: flex
- flex-direction: column
- align-items: center
- padding: 80px 0
- max-width: 1200px
- background-color: #1a1a2e
- gap: 24px

### Heading (H1)
- font-family: Inter
- font-size: 56px
- font-weight: 700
- line-height: 1.1
- color: #ffffff
- letter-spacing: -0.02em
- margin-bottom: 16px

### Body
- font-family: Inter
- font-size: 18px
- line-height: 1.6
- color: #a0a0b0
```

3. Cross-reference with `.agent/design-system.md` — flag any values that differ from globals.

**Output**: A complete Section Brief with all CSS values.

---

## Step 2: Apply to Code

**Process**:
1. Locate the component file for this section.
2. Map Section Brief values to the codebase's styling approach. Adapt the syntax:
   - **Vanilla CSS**: Update matching selectors directly.
   - **Tailwind**: Convert to utility classes (e.g., `padding: 80px 0` → `py-20`).
   - **CSS Modules / Styled Components**: Update style objects.
3. Apply **exact values** from the Brief. Do not approximate.
   - Use the captured value (e.g., `padding: 80px`), not a "close" design token (e.g., `padding: 5rem`) unless the token equals the captured value.
4. Work **outside-in**: Container first, then child elements.

---

## Step 3: Gap Analysis (Visual Comparison)

**When called**: After all sections have been code-applied, or for ad-hoc fixes.

**Inputs**:
- Target visual: `[page]-[0n]-[section]-visual.png`
- Current Build screenshot (from user)
- Section Brief (from Step 1)

**Process**:
> "List the top 5 visual discrepancies between TARGET and CURRENT BUILD.
> For each, state:
> 1. **What** is wrong (e.g., 'Hero heading size').
> 2. **Target value** (from Brief: e.g., '56px').
> 3. **Current value** (estimated or inspected).
>
> Prioritise hard data mismatches over subjective differences."

**Output**: Numbered Discrepancy List.

---

## Step 4: Fix

1. Fix **only** items in the Discrepancy List.
2. Use **exact values** from the data files.
3. If a CSS cascade issue is suspected (e.g., padding on wrong element), trace the container hierarchy before changing values.
4. Update `.agent/design-system.md` if a new global pattern is discovered.

---

## Step 5: Verify

1. Ask user to re-screenshot at same viewport/scroll.
2. Re-run Gap Analysis (Step 3).
3. **Done when**: All discrepancies resolved or remaining diffs ≤ 2px.
4. **Not done**: Return to Step 4 with updated list.
