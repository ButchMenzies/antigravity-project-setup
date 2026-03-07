---
name: Build Component
description: Reuse-first component creation — never duplicate, always check existing
triggers: [new component, UI component, build component, create component, shared component]
track_types: [feature, refactor]
mcps: []
---

# Build Component

## When to Use

When the track involves creating or significantly modifying a UI component. This skill's primary job is **preventing duplication** — the agent must check for existing components before creating new ones.

## Plan Checklist

### Pre-build (MANDATORY)

- [ ] List all existing components in `components/` (or equivalent directory)
- [ ] Search for components with similar purpose — cards, buttons, modals, forms, etc.
- [ ] If a similar component exists: extend it with props/variants instead of creating a new one
- [ ] If creating new: confirm it belongs in `components/ui/` (shared) vs `components/features/` (feature-specific)

### Implementation

- [ ] Use existing design tokens and Tailwind classes — avoid arbitrary values when tokens exist
- [ ] Support dark mode if the project uses it
- [ ] Handle all states: default, hover, focus, active, disabled
- [ ] Include proper ARIA attributes for accessibility
- [ ] Ensure keyboard navigation works (Tab, Enter, Escape as appropriate)
- [ ] Make responsive: test at mobile (320px+), tablet (768px+), desktop (1024px+)

### Props & API

- [ ] Use TypeScript interface for props — no `any` types
- [ ] Provide sensible defaults for optional props
- [ ] Use `variants` pattern for visual variations (not separate components)
- [ ] Forward refs if the component wraps a native element

### Verification

- [ ] Component renders correctly in all states
- [ ] No duplicate components created — checked existing first
- [ ] Responsive at all breakpoints
- [ ] Keyboard accessible

## Common Mistakes

1. **Creating a new component when one already exists** — the #1 mistake. Always scan `components/` first
2. **Hardcoding colors/spacing instead of using design tokens** — creates inconsistency across the app
3. **Forgetting disabled and loading states** — components break when used in real forms
4. **No keyboard support** — buttons that only work with mouse clicks fail accessibility
5. **Building a "page-specific" component** that should be shared — if it could be used elsewhere, put it in `components/ui/`
