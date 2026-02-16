---
name: react-shadcn-dashboard
description: Patterns for building admin/config dashboard pages with shadcn/ui, metric cards, data grids, and consistent styling.
---

# React + shadcn Dashboard Patterns

Build consistent admin and configuration dashboard pages using our established component patterns.

## Use this skill when

- Building a new admin configuration page (e.g., `/admin/*`)
- Creating a dashboard with stat cards and data grids
- Adding metric/summary sections to any page
- Building card-based layouts with icons and badges

## Do not use this skill when

- Building wizard/form-heavy pages (use form patterns instead)
- Working on backend/API logic only

## Instructions

### Step 1: Page Structure

Every dashboard page follows this structure:

```tsx
export default function AdminPageName() {
  return (
    <div className="p-6 md:p-8 lg:p-12 space-y-6">
      {/* Header */}
      <div>
        <h1 className="text-3xl font-bold tracking-tight">Page Title</h1>
        <p className="text-muted-foreground mt-1">Subtitle description</p>
      </div>

      {/* Summary Stats Row */}
      <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
        <StatCard label="Label" value="42" icon={Icon} color="blue" />
      </div>

      {/* Info Banner (optional) */}
      <div className="rounded-lg border bg-gradient-to-br from-blue-500/10 to-transparent border-blue-500/20 p-4">
        <p className="text-sm text-muted-foreground">Contextual info...</p>
      </div>

      {/* Content Grid */}
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-4">
        {items.map((item) => (
          <ItemCard key={item.id} item={item} />
        ))}
      </div>
    </div>
  );
}
```

### Step 2: Stat Cards

Use the `MetricCard` component or inline pattern:

```tsx
import MetricCard from "@/components/MetricCard";

<MetricCard
  title="Sessions Completed"
  value={`${count}/${total}`}
  icon={CheckCircle2}
  subtitle="Past week"
  gradient="bg-gradient-to-br from-green-500/10 to-transparent"
  labelColor="text-green-600"
  borderColor="border-green-500/20"
/>
```

### Step 3: Data Cards with Icons

Map Lucide icon names from data to components:

```tsx
import { Wrench, Heart, Users, Brain, Shield, Star } from "lucide-react";

const iconMap: Record<string, React.ComponentType<any>> = {
  Wrench, Heart, Users, Brain, Shield, Star,
};

function ItemCard({ item }: { item: ItemType }) {
  const Icon = iconMap[item.icon] || Users;
  return (
    <Card className={cn(
      "hover-elevate transition-all duration-200",
      "bg-gradient-to-br to-transparent",
      item.gradient
    )} data-testid={`card-admin-${item.key}`}>
      <CardContent className="p-5 space-y-4">
        <div className="flex items-center gap-3">
          <div className={cn("p-2 rounded-lg", item.gradient)}>
            <Icon className={cn("w-5 h-5", item.color)} />
          </div>
          <h3 className="font-semibold">{item.name}</h3>
        </div>
        <p className="text-sm text-muted-foreground">{item.description}</p>
      </CardContent>
    </Card>
  );
}
```

### Step 4: Color Palette

Use these gradient/border combos consistently:

| Color | Gradient | Border | Text |
|-------|----------|--------|------|
| Blue | `from-blue-500/10` | `border-blue-500/20` | `text-blue-600` |
| Green | `from-green-500/10` | `border-green-500/20` | `text-green-600` |
| Purple | `from-purple-500/10` | `border-purple-500/20` | `text-purple-600` |
| Yellow | `from-yellow-500/10` | `border-yellow-500/20` | `text-yellow-600` |
| Red | `from-red-500/10` | `border-red-500/20` | `text-red-600` |

### Step 5: Test IDs

Always add `data-testid` attributes for browser testing:

- Cards: `data-testid={`card-admin-${item.key}`}`
- Buttons: `data-testid="button-<action>"`
- Inputs: `data-testid="input-<field>"`

## Checklist

- [ ] Page has header with title + subtitle
- [ ] Summary stats use MetricCard or equivalent
- [ ] Cards use `hover-elevate` class
- [ ] Gradients follow the color palette table
- [ ] All interactive elements have `data-testid`
- [ ] Responsive grid: `grid-cols-1 lg:grid-cols-2`

## References

- `client/src/pages/admin/personal.tsx` — Full CRUD admin page example
- `client/src/pages/admin/support.tsx` — Read-only config dashboard
- `client/src/pages/admin/vision.tsx` — Multi-section dashboard
- `client/src/components/MetricCard.tsx` — Reusable metric card
