---
description: Compare your existing build against a target design — apply captured data to fix discrepancies.
---

# Compare Site Workflow

**Role**: ORCHESTRATOR — Compare an existing build against captured target data, apply fixes, then verify visually.

> **Prerequisite**: Capture data must exist in `.agent/targets/[page]/`. If not, the agent will redirect to `/capture-target`.

---

## Phase 1: Load the Source of Truth

### Step 1: Select Page
> "Which page are we comparing? (e.g., Home, About)"

### Step 2: Check for Capture Data
**Agent Action**: Check if `.agent/targets/[page]/` exists and contains data files.

- **IF data exists**: Load it and proceed.
- **IF no data exists**: "No capture data found for **[Page Name]**. You need to run `/capture-target` first to gather the design data. Want me to start that now?"

### Step 3: Load Target Data
1. Read `.agent/targets/[page]/metadata.md` for viewport width and URL.
2. List all files in `.agent/targets/[page]/` to map the captured sections.
3. Read `.agent/design-system.md` if it exists — these are global tokens.

### Step 4: Map the Codebase
Before making changes, understand the project structure:
- Identify the CSS/styling approach (vanilla CSS, Tailwind, CSS Modules, styled-components).
- Locate the component files for each captured section.
- Identify the global stylesheet or theme file where tokens live.

---

## Phase 2: Code Pass (No Browser Needed)

**Goal**: Apply 90% of the design from captured data alone.

### Step 5: Global Tokens First
Read ALL data files across ALL sections. Extract repeating values into `.agent/design-system.md`:
- **Fonts**: font-family, base sizes for H1–H6 and body.
- **Colours**: backgrounds, text colours, accent colours.
- **Spacing**: common padding/margin values (e.g., section padding is always `80px 0`).
- **Borders**: common border-radius, shadows.

Apply these global tokens to the theme/global stylesheet first.

### Step 6: Section-by-Section Code Application
For each section (in order: 01, 02, 03...):

1. **Read the data files** for this section (container + elements).
2. Call the `visual-qa` skill **Step 1** to build a Section Brief from the data.
3. **Find the matching component** in the codebase.
4. **Apply values directly**:
   - Container: `max-width`, `padding`, `margin`, `display`, `flex-direction`, `gap`, `text-align`, `background-color`.
   - Elements: `font-size`, `font-weight`, `line-height`, `color`, `letter-spacing`, `border-radius`, `box-shadow`, etc.
5. **DO NOT open a browser yet**. Just write the code.

> After applying all sections, move to Phase 3.

---

## Phase 3: Visual Verification

### Step 7: User Screenshots
The user screenshots are required for verification because the Agent cannot perfectly control browser viewport width.

> "Please open `localhost:[port]` in your browser.
> Set the viewport to **[width from metadata]** using the same Dev Tools setup from `/capture-target`.
> Screenshot each section and upload them in order (max 5 per round)."

### Step 8: Section-by-Section Visual Check
For each section:
1. Call the `visual-qa` skill **Step 3** (Gap Analysis) comparing:
   - Target visual (`-visual.png`) against the user's Current Build screenshot.
   - Cross-reference with the data files for hard values.
2. **IF discrepancies found**: Fix in code, ask user to re-screenshot that section.
3. **IF clean** (within 2px tolerance): Move to next section.

> Max **3 iterations** per section before moving on.

---

## Phase 4: Completion

### Step 9: Summary
> "Comparison complete for **[Page Name]**.
> - **Code Pass**: [N] sections updated from captured data.
> - **Visual Fixes**: [M] adjustments after visual verification.
> - **Remaining Issues**: [list, if any]
>
> Run `/compare-site` again after fixing remaining issues, or `/capture-target` for another page."
