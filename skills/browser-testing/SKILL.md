---
name: browser-testing
description: Standardized browser verification workflow for testing UI changes using the browser subagent.
---

# Browser Testing Workflow

Consistent process for verifying UI changes using the Antigravity browser subagent tool.

## Use this skill when

- Verifying a new page or component renders correctly
- Testing interactive features (buttons, forms, navigation)
- Capturing screenshots for walkthroughs
- Validating responsive layouts
- Checking for visual regressions after changes

## Do not use this skill when

- Running automated unit tests (use terminal commands)
- Testing backend-only changes

## Instructions

### Step 1: Ensure Dev Server is Running

Before testing, check that the app is serving on port 5010:

```bash
# Check if server is running
lsof -i :5010

# Start if not running
npm run dev
```

### Step 2: Navigate and Capture

Use the `browser_subagent` tool with a clear, detailed task:

```
Task: Navigate to http://localhost:5010/<page> and verify:
1. Page loads without errors
2. [Specific element] is visible
3. [Interactive feature] works when clicked
4. Capture a screenshot of the final state

Return: Description of what you observed, any errors, and confirmation of each check.
```

### Step 3: Test Checklist by Page Type

**Dashboard/Admin pages:**
- [ ] Header with title and subtitle renders
- [ ] Stat cards show correct values
- [ ] Data cards render with icons
- [ ] Gradient colors are visible (not white)
- [ ] Hover effects work on cards
- [ ] Page is responsive (check at different widths if needed)

**Form pages:**
- [ ] All form fields render with labels
- [ ] Validation errors appear on invalid submit
- [ ] Submit button disables during operation
- [ ] Success toast appears on save
- [ ] Error toast appears on failure

**Data display pages:**
- [ ] Data loads and displays correctly
- [ ] Loading state shows while fetching
- [ ] Empty state shows when no data
- [ ] Filters/tabs switch content correctly

### Step 4: Capture Evidence for Walkthroughs

Name screenshots descriptively:

```
RecordingName: "admin_support_test"  — for recordings
Screenshot naming: <page>_<state>_<timestamp>
```

Always take screenshots of:
1. Full page load (top section)
2. Bottom section (scroll down)
3. Interactive state changes (before/after clicks)

### Step 5: Common Issues to Watch For

| Issue | What to Check |
|-------|-------------|
| Blank page | Check browser console for JS errors |
| Missing data | Verify API endpoint and auth |
| Broken styles | Check Tailwind classes are valid |
| Auth redirect | Ensure test user is logged in |
| 404 on route | Check router config in `main.tsx` |

### Step 6: Login Flow (if needed)

If the page requires authentication:

```
Task: Navigate to http://localhost:5010/login, enter:
- Email: <test email>
- Password: <test password>
Then navigate to the target page.
```

## Recording Tips

- Use `RecordingName` with lowercase and underscores: `"review_page_test"`
- Keep recordings focused — one feature per recording
- For walkthrough screenshots, save to the artifacts directory

## References

- `.agent/AGENT.md` — Dev server port (5010) and startup instructions
- `client/src/main.tsx` — All available routes
