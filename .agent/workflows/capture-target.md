---
description: Guide the user to capture high-quality, standardized reference images for the Visual QA process using an interactive, agent-driven loop.
---

# Capture Target Workflow

**Role**: INPUT — Gather and store an exhaustive "Source of Truth" so the Agent can rebuild the design from data alone, without guessing.

> **Beginner Friendly**: This workflow includes step-by-step instructions. No prior Dev Tools experience required.

---

## Phase 1: Setup

### Step 1: Context
> "**1. What page are we capturing?** (e.g., Home, About, Pricing)"
> "**2. What is the URL of the live site** to capture?"

### Step 2: Prepare Storage
Create the directory and metadata file:
```
.agent/targets/[page]/metadata.md
```
```markdown
# Target: [Page Name]
- URL: [url]
- Viewport: 1440px
- Captured: [date]
```

### Step 3: Browser Configuration

> [!IMPORTANT]
> Dev Tools must stay **OPEN** for the entire session. Opening or closing them changes the content width.

**Instructions** (read to the user step by step):

1.  **Open Dev Tools**: Right-click → **Inspect** (or `Cmd+Option+I` / `F12`).
2.  **Dock to Right**: Click the **⋮ menu** (top-right of Dev Tools panel) → select the **Dock to right** icon.
3.  **Set Width to 1440px**: Drag the vertical divider between page and Dev Tools. Watch the dimension indicator at the top-right of the content area. Stop at **`1440`**.
4.  **Reset Zoom**: `Cmd+0` (Mac) / `Ctrl+0` (Windows).
5.  **Keep Dev Tools open** for the rest of this session.

---

## Phase 2: The Smart Capture Loop

Repeat the **A → B → C** loop for each section of the page.

> [!IMPORTANT]
> **One section at a time.** The user can upload a maximum of **5 images per round**. You may run **unlimited rounds** — a page with 20 sections might take 4+ rounds. Never request more than 5 images in a single prompt. If a section needs 6+ data files, split the requests across two rounds.

> [!NOTE]
> **File Naming**: The user will upload screenshots with default names (e.g., `Screenshot 2026-02-19...`). The **agent must rename** all files to the structured convention (e.g., `home-01-hero-visual.png`, `home-01-hero-data-container.png`) when saving to `.agent/targets/[page]/`.

### A. Request Visual Screenshot

> "Scroll to the **[Section Name]** area.
> Screenshot this section (`Cmd+Shift+4` / `Win+Shift+S`).
> Upload it now — I'll rename the file for you."

> [!NOTE]
> **Text Content**: After viewing the screenshot, note down the exact text content for this section — headings, body copy, button labels, and list items. If any text is hard to read from the screenshot, ask the user to confirm it.

### B. Analyse Visual & Request Data (Layered Extraction)

**Agent Action**: Look at the visual. Identify the UI patterns present. Then request data in **two layers**:

---

#### Layer 1: The Section Container (ALWAYS capture this)

Every section has an outermost wrapper. **Always** ask the user to inspect it.

> "Please inspect the **outermost container/section wrapper** for this area."

| Property | Where to find it | Why |
|:---|:---|:---|
| `height` (rendered) | Computed → Box Model (blue area, bottom number) | Section height — becomes `min-height` during build |
| `padding` (all sides) | Computed → Box Model (green area) | Vertical/horizontal breathing room |
| `margin` (all sides) | Computed → Box Model (orange area) | Space between sections |
| `max-width` | Computed → filter "max-width" | Content constraint |
| `background-color` | Computed → filter "background" | Section colour |
| `display` | Computed → filter "display" | `flex`, `grid`, or `block` |
| `flex-direction` | Computed → filter "flex-direction" | Row vs column layout |
| `justify-content` | Computed → filter "justify" | Horizontal distribution |
| `align-items` | Computed → filter "align" | Vertical alignment |
| `gap` | Computed → filter "gap" | Space between children |
| `text-align` | Computed → filter "text-align" | Left, center, right |

> [!TIP]
> **Inner content wrapper**: Many sections have an outer `<section>` (full-width background) and an inner `<div>` that constrains the content (e.g., `max-width: 1200px; margin: 0 auto`). If the content doesn't span the full viewport, ask the user to also inspect the **inner wrapper** for its `max-width`, `margin`, and `padding`.

> "Screenshot the **full Computed panel** (Box Model + properties). Upload it now — I'll rename the file for you."

---

#### Layer 2: Key Elements (based on what you see)

After capturing the container, ask for specific elements based on what's visible.

| IF you see... | Inspect this element | Properties to capture |
|:---|:---|:---|
| **Heading** (H1–H6) | The heading itself | `font-family`, `font-size`, `font-weight`, `line-height`, `color`, `letter-spacing`, `text-transform`, `margin` (bottom especially) |
| **Body Text** | A `<p>` element | `font-family`, `font-size`, `line-height`, `color`, `max-width` (if set) |
| **Button / CTA** | The `<button>` or `<a>` | `padding`, `border-radius`, `background-color`, `color`, `font-size`, `font-weight`, `border` |
| **Hover / Active State** | Same element, while hovered | `background-color`, `color`, `transform`, `opacity`, `transition` — ask user to hover and screenshot |
| **Card / Tile** | One card element | `padding`, `border-radius`, `background-color`, `box-shadow`, `border` |
| **Sub-element with distinct background** | A child inside a card/section with its own colour | `padding`, `background-color`, `border-radius`, `border`, `font-style` |
| **Grid / Card Layout** | The grid/flex parent | `display`, `grid-template-columns`, `gap`, `flex-wrap` |
| **Image** | The `<img>` | `width`, `height`, `object-fit`, `border-radius` |
| **Video** | The `<video>` | `width`, `height`, `object-fit`, `aspect-ratio` |
| **Navigation / Header** | The `<nav>` or `<header>` | `height`, `padding`, `background-color`, `position` (`fixed`/`sticky`/`relative`), `z-index` |
| **Overlay / Gradient** | The overlay `<div>` | `background` (gradient value), `opacity` |
| **List / Links** | A `<ul>` or `<a>` group | `gap` (between items), `font-size`, `font-weight`, `color`, `text-decoration` |

> [!IMPORTANT]
> **Media Clarification**: A screenshot of a video looks identical to a static image.
> If you see a large photo or full-bleed visual, **always ask**:
> *"Is that a static image or a video? I need to know so I capture the right element."*
> - **If image**: Capture `<img>` dimensions and `object-fit`.
> - **If video**: Capture `<video>` dimensions, and note if it autoplays/loops.

> **How to Inspect an Element** (tell user if needed):
> 1. Click the **Inspect cursor** (🔍 top-left of Dev Tools, or `Cmd+Shift+C`).
> 2. Hover over the element — it highlights.
> 3. **Click** to select it.
> 4. Click the **Computed** tab (next to "Styles").
> 5. You'll see the **Box Model** (coloured rectangles) and CSS properties below.
> 6. **Tip**: Use the **Filter** box at the top of the Computed panel to search for a specific property (e.g., type "font" to see all font properties at once).
> 7. **Screenshot the panel** and upload it.

> **Agent Prompt**: "I can see [describe section]. Please inspect:
> 1. The **section container** (Data file: `[page]-[0n]-[section]-data-container.png`)
> 2. The **[specific element]** (Data file: `[page]-[0n]-[section]-data-[element].png`)
> 3. The **[another element]** (Data file: `[page]-[0n]-[section]-data-[element].png`)"

### C. Validate & Continue

- **IF readable**: "Got it. I can see [summarise key values]. Moving on."
- **IF blurry/wrong tab**: "I can't read the values. Make sure you're on the **Computed** tab and try again."
- **IF missing data**: "I still need the [element] data. Can you inspect that too?"

> "Ready for the next section? Scroll down, or say **done** if we've reached the bottom."

---

## Phase 3: Page-Level Data

### Step 4: Fonts & Meta
Before completing the capture, gather page-level information:

> "Almost done. I need a few more things:
> 1. **Fonts**: In Dev Tools, click the **Sources** or **Network** tab. Filter by 'font'. What font files are loading? Or check the `<head>` for any `<link>` tags with `fonts.googleapis.com`.
> 2. **Page title**: What does the browser tab say? (Or inspect `<title>` in the `<head>`.)
> 3. **Favicon**: Right-click the browser tab icon — is there a custom favicon?"

Save font URLs and meta info to `metadata.md`.

---

## Phase 4: Completion

### Summary & Handoff

> "Capture complete for **[Page Name]**.
> I have **[N] sections** with exhaustive data in `.agent/targets/[page]/`.
>
> **Next step**:
> - Run `/recreate-site` to build this site from scratch.
> - Run `/compare-site` to refine an existing build against this target."
