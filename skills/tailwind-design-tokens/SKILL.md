---
name: tailwind-design-tokens
description: Project-specific Tailwind design tokens — gradients, borders, hover effects, spacing, and typography conventions.
---

# Tailwind Design Tokens

Our project's visual language codified as reusable Tailwind class patterns. Ensures visual consistency across all pages.

## Use this skill when

- Building any new page or component
- Choosing colors for cards, badges, or sections
- Adding hover/active effects to interactive elements
- Styling metric cards, data cards, or info banners

## Do not use this skill when

- Working on backend/API code
- Modifying third-party component internals

## Instructions

### Step 1: Card Gradient Pattern

All cards use this gradient pattern:

```
bg-gradient-to-br from-{color}-500/10 to-transparent border-{color}-500/20
```

Apply via className:

```tsx
<div className="rounded-lg border bg-gradient-to-br from-purple-500/10 to-transparent border-purple-500/20 p-4">
  {/* content */}
</div>
```

### Step 2: Color Palette

| Semantic Use | Color | Gradient | Border | Text |
|-------------|-------|----------|--------|------|
| Primary / Actions | Purple | `from-purple-500/10` | `border-purple-500/20` | `text-purple-600` |
| Success / Completed | Green | `from-green-500/10` | `border-green-500/20` | `text-green-600` |
| Warning / Adjust | Yellow | `from-yellow-500/10` | `border-yellow-500/20` | `text-yellow-600` |
| Info / Improve | Blue | `from-blue-500/10` | `border-blue-500/20` | `text-blue-600` |
| Danger / Error | Red | `from-red-500/10` | `border-red-500/20` | `text-red-600` |
| Neutral / Muted | Gray | `from-muted/50` | `border-border` | `text-muted-foreground` |

### Step 3: Interactive Effects

```
hover-elevate        → Standard hover lift (defined in global CSS)
active-elevate-2     → Press-down effect
transition-all duration-200 → Smooth transitions
cursor-pointer       → All clickable elements
```

Card with full interaction:

```tsx
<Card className={cn(
  "hover-elevate transition-all duration-200 cursor-pointer",
  "bg-gradient-to-br to-transparent",
  item.gradient
)}>
```

### Step 4: Typography Scale

| Element | Classes |
|---------|---------|
| Page title | `text-3xl font-bold tracking-tight` or `text-4xl font-bold tracking-tight` |
| Section heading | `text-2xl font-semibold` |
| Card title | `font-semibold` or `text-lg font-medium` |
| Section label | `text-purple-600 font-semibold text-xs uppercase tracking-wider` |
| Body text | `text-sm text-muted-foreground` |
| Subtle hint | `text-xs opacity-70` or `text-xs text-muted-foreground` |

### Step 5: Spacing Conventions

| Context | Classes |
|---------|---------|
| Page padding | `p-6 md:p-8 lg:p-12` |
| Section gap | `space-y-6` |
| Card padding | `p-4` or `p-5` |
| Grid gap | `gap-4` |
| Between header and content | `mb-8` |
| Between label and content | `mb-4` |

### Step 6: Responsive Grid Patterns

| Content | Pattern |
|---------|---------|
| Stat cards (4) | `grid grid-cols-2 md:grid-cols-4 gap-4` |
| Data cards | `grid grid-cols-1 lg:grid-cols-2 gap-4` |
| Period selector | `grid grid-cols-1 md:grid-cols-3 gap-4` |
| Form fields (full width) | `max-w-3xl grid grid-cols-1 gap-4` |

### Step 7: Badge Styling

```tsx
<Badge variant="outline" className="border-purple-600 text-purple-600">
  label
</Badge>
```

Match badge color to the card's semantic color (see palette table).

## Checklist

- [ ] Cards use gradient pattern (not flat backgrounds)
- [ ] Colors match semantic meaning from palette table
- [ ] Interactive elements have `hover-elevate` and `cursor-pointer`
- [ ] Page uses standard spacing (`p-6 md:p-8 lg:p-12 space-y-6`)
- [ ] Section labels use uppercase tracking-wider pattern
- [ ] Grids are responsive with proper breakpoints
