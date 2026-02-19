---
description: Recreate an existing website from scratch — capture its design data, then build it in your chosen tech stack.
---

# Recreate Site Workflow

**Role**: PIPELINE — Orchestrate the full capture → build → verify cycle to recreate a website from scratch.

---

## Phase 1: Setup

### Step 1: Context
> "**1. What is the URL of the site** you want to recreate?"
> "**2. What tech stack** are you building in?"
>    - Static HTML/CSS
>    - React + Vite
>    - Next.js
>    - Other (specify)

### Step 2: Project Setup
Based on the chosen stack:
- **Static HTML/CSS**: Create `index.html` and `styles.css` in the project directory.
- **React + Vite**: Scaffold with `npx create-vite@latest` (or use existing project).
- **Next.js**: Scaffold with `npx create-next-app@latest` (or use existing project).

> [!NOTE]
> The captured design data is **technology-agnostic**. The same CSS values apply regardless of stack — only the file structure and syntax differs.

---

## Phase 2: Capture

### Step 3: Check for Existing Data
**Agent Action**: Check if `.agent/targets/[page]/` already exists and contains data files.

- **IF data exists**: "I found existing capture data for **[Page Name]** with **[N] sections**. Do you want to **use this data** or **re-capture** the site?"
- **IF no data exists**: "No capture data found. Let's capture the site now."

### Step 4: Run Capture (if needed)
**Guide the user** through `/capture-target` to gather data from the live site. The agent orchestrates the capture but the **user** does all browser/DevTools work.

- Capture **every section** of the page, top to bottom.
- One section at a time, max 5 screenshot uploads per message.
- All data saved to `.agent/targets/[page]/`.

> [!IMPORTANT]
> Do not proceed to Phase 3 until ALL sections have been captured. The build phase needs complete data.

---

## Phase 3: Build (No Browser Needed)

### Step 5: Global Tokens
Read ALL data files across ALL sections. Extract repeating values into `.agent/design-system.md`:
- **Fonts**: font-family, base sizes for H1–H6 and body.
- **Colours**: backgrounds, text colours, accent colours.
- **Spacing**: common padding/margin values (e.g., section padding is always `80px 0`).
- **Borders**: common border-radius, shadows.
- **Layout**: common max-width, content centering patterns.

Apply these to the global stylesheet / theme file first.

### Step 6: Section-by-Section Build
For each captured section (in page order: 01, 02, 03...):

1. **Read the data files** for this section (container + elements).
2. Call the `visual-qa` skill **Step 1** to build a Section Brief from the data.
3. **Generate the HTML structure** (or component, depending on stack).
4. **Apply exact CSS values** from the Brief.
5. Work **outside-in**: Container first, then child elements.

> [!IMPORTANT]
> Use the **exact captured values**. Do not approximate. `padding: 80px` stays as `80px`, not `5rem`, unless the project uses a token system where `5rem = 80px`.



> [!CAUTION]
> **Section height rule**: Every captured section has a rendered height. Apply this as `min-height` on the section container. If the target is 1829px tall, your section must be approximately 1829px tall. Do NOT substitute with arbitrary spacing.

> [!WARNING]
> **Tailwind / utility class trap**: Do NOT use arbitrary spacing classes (`py-12`, `space-y-8`, `gap-8`) when exact pixel values are captured. Use bracket notation: `min-h-[1829px]`, `gap-[200px]`, `py-[80px]`. The captured data is the source of truth, not framework defaults.

**Vertical sanity check**: After building each section, compare your output's expected height against the captured height. If the captured height is 1829px and your section would render at ~400px, stop — check gaps, padding, content sizing, and min-height.

### Step 7: Serve Locally
Start the dev server and confirm the page loads:
- **Static**: `python3 -m http.server 8080` or similar.
- **React + Vite**: `npm run dev`.
- **Next.js**: `npm run dev`.

---

## Phase 4: Verify

### Step 8: User Screenshots
> "Please open the rebuilt site in your browser.
> Set the viewport to **1440px** using the same Dev Tools setup from `/capture-target`.
> Screenshot each section from top to bottom and upload them."

### Step 9: Gap Analysis
For each section:
1. Compare the user's screenshot against the target visual (`-visual.png`).
2. Cross-reference with the data files for hard values.
3. **IF discrepancies found**: Fix in code, ask user to re-screenshot that section.
4. **IF clean** (within 2px tolerance): Move to next section.

> Max **3 iterations** per section before moving on.

---

## Phase 5: Completion

### Step 10: Summary
> "Site recreation complete for **[Page Name]**.
> - **Sections built**: [N] from captured data.
> - **Visual fixes**: [M] adjustments after verification.
> - **Remaining issues**: [list, if any]
>
> To refine further, run `/compare-site` against the target data."
