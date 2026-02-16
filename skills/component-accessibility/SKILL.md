---
name: component-accessibility
description: WCAG accessibility checklist for shadcn/ui components — keyboard nav, ARIA, contrast, focus management.
---

# Component Accessibility (WCAG + shadcn/ui)

Accessibility patterns and checklist tailored to our shadcn/ui + Tailwind stack. Ensures WCAG 2.1 AA compliance.

## Use this skill when

- Building new interactive components (forms, modals, dropdowns)
- Reviewing existing components for accessibility
- Adding keyboard navigation
- Working with color-only indicators
- Creating data visualizations or charts

## Do not use this skill when

- Working on backend/API code
- Styling static content that has no interactive elements

## Instructions

### Step 1: Semantic HTML First

Always use the correct HTML element before reaching for ARIA:

| Need | Use | Not |
|------|-----|-----|
| Navigation | `<nav>` | `<div>` |
| Button action | `<button>` | `<div onClick>` |
| Link to page | `<a href>` or `<Link>` | `<button>` with navigate |
| List of items | `<ul>/<li>` | Nested `<div>` |
| Heading | `<h1>-<h6>` | `<div className="font-bold">` |
| Form field | `<label>` + `<input>` | `<span>` + `<input>` |

### Step 2: Form Accessibility

shadcn/ui forms require explicit labeling:

```tsx
// ✅ CORRECT — Label associated with input
<div className="space-y-2">
  <Label htmlFor="email">Email address</Label>
  <Input id="email" type="email" placeholder="you@example.com" />
</div>

// ❌ WRONG — No label association
<Input placeholder="Email" />
```

For error states:

```tsx
<div className="space-y-2">
  <Label htmlFor="password">Password</Label>
  <Input
    id="password"
    type="password"
    aria-invalid={!!errors.password}
    aria-describedby={errors.password ? "password-error" : undefined}
  />
  {errors.password && (
    <p id="password-error" className="text-sm text-red-500" role="alert">
      {errors.password}
    </p>
  )}
</div>
```

### Step 3: Interactive Elements

**Icon-only buttons** must have accessible labels:

```tsx
// ✅ CORRECT
<Button variant="ghost" size="icon" aria-label="Close dialog">
  <X className="w-4 h-4" />
</Button>

// ❌ WRONG — No label
<Button variant="ghost" size="icon">
  <X className="w-4 h-4" />
</Button>
```

**Toggle/checkbox states:**

```tsx
<button
  role="switch"
  aria-checked={isEnabled}
  aria-label="Enable notifications"
  onClick={() => setIsEnabled(!isEnabled)}
>
  {/* visual indicator */}
</button>
```

### Step 4: Color and Contrast

- **Text contrast:** Minimum 4.5:1 ratio for normal text, 3:1 for large text
- **Never use color alone** to convey meaning:

```tsx
// ❌ WRONG — Color is the only indicator
<div className={status === "error" ? "text-red-500" : "text-green-500"}>
  {status}
</div>

// ✅ CORRECT — Color + icon + text
<div className={cn("flex items-center gap-2", statusColors[status])}>
  {status === "error" ? <AlertCircle className="w-4 h-4" /> : <Check className="w-4 h-4" />}
  <span>{statusLabel}</span>
</div>
```

### Step 5: Focus Management

**Modals/dialogs:** Focus should trap inside and restore on close:

```tsx
// shadcn Dialog handles this automatically
<Dialog>
  <DialogTrigger asChild>
    <Button>Open</Button>
  </DialogTrigger>
  <DialogContent>
    {/* Focus is auto-trapped here */}
  </DialogContent>
</Dialog>
```

**Custom focus patterns:**

```tsx
// Focus the first input when a section opens
const inputRef = useRef<HTMLInputElement>(null);
useEffect(() => {
  if (isOpen) inputRef.current?.focus();
}, [isOpen]);
```

### Step 6: Keyboard Navigation

All interactive elements must be keyboard-accessible:

| Element | Expected Keys |
|---------|--------------|
| Button | Enter, Space |
| Link | Enter |
| Tab/Accordion | Arrow keys |
| Dialog | Escape to close |
| Dropdown | Arrow keys, Escape |
| Checkbox | Space |

Test by tabbing through the page — can you reach and activate every interactive element?

### Step 7: Motion and Animations

Respect `prefers-reduced-motion`:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

## Pre-Delivery Checklist

- [ ] All form inputs have associated `<Label>`
- [ ] Icon-only buttons have `aria-label`
- [ ] Color is not the only state indicator
- [ ] Focus order is logical (tabbing flows naturally)
- [ ] Dialogs trap focus and restore on close
- [ ] Error messages use `role="alert"`
- [ ] Images have `alt` text (or `alt=""` for decorative)
- [ ] Headings follow hierarchy (h1 → h2 → h3)
- [ ] No autoplaying animations without reduced-motion support

## References

- shadcn/ui docs: https://ui.shadcn.com
- WCAG 2.1 Quick Reference: https://www.w3.org/WAI/WCAG21/quickref/
- `client/src/components/ui/` — Our shadcn component library
