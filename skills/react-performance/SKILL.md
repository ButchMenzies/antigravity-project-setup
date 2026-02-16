---
name: react-performance
description: React performance optimization — memoization, code splitting, re-render prevention, and waterfall elimination.
---

# React Performance Optimization

Performance patterns for React + Vite apps. Based on Vercel's 45 performance rules, adapted for our stack.

## Use this skill when

- Profiling or optimizing slow components
- Adding code splitting or lazy loading
- Reviewing components for unnecessary re-renders
- Optimizing data fetching patterns
- Working on pages with large lists or complex calculations

## Do not use this skill when

- Building a first draft (premature optimization)
- Performance is already acceptable

## Instructions

### Step 1: Eliminate Waterfall Requests (CRITICAL)

**Parallel fetches:** Never chain independent API calls.

```tsx
// ❌ WRONG — Sequential waterfall
const users = await fetch("/api/users").then(r => r.json());
const plans = await fetch("/api/plans").then(r => r.json());

// ✅ RIGHT — Parallel
const [users, plans] = await Promise.all([
  fetch("/api/users").then(r => r.json()),
  fetch("/api/plans").then(r => r.json()),
]);
```

**React Query parallel queries:**

```tsx
// Multiple queries run in parallel automatically
const { data: plan } = useQuery({ queryKey: ["/api/plan"] });
const { data: sessions } = useQuery({ queryKey: ["/api/sessions"] });
```

### Step 2: Memoize Expensive Computations

```tsx
// ✅ useMemo for sorting/filtering/aggregation
const sortedSessions = useMemo(() => {
  return sessions
    .filter(s => s.status === "completed")
    .sort((a, b) => new Date(b.date).getTime() - new Date(a.date).getTime());
}, [sessions]);

// ✅ useCallback for functions passed as props
const handleSelect = useCallback((id: number) => {
  setSelectedId(id);
}, []);
```

**When NOT to memoize:**
- Simple expressions (string concatenations, boolean checks)
- Values not passed to child components
- Components that rarely re-render

### Step 3: Prevent Unnecessary Re-renders

```tsx
// ✅ Use React.memo for pure display components
const SessionCard = React.memo(({ session }: { session: Session }) => (
  <Card>
    <h3>{session.title}</h3>
    <p>{session.notes}</p>
  </Card>
));

// ✅ Use derived state instead of raw subscriptions
// Instead of subscribing to an entire object:
const isActive = useMemo(() => plan?.status === "active", [plan?.status]);
```

### Step 4: Code Splitting with Lazy Loading

```tsx
import { lazy, Suspense } from "react";

// ✅ Lazy load heavy pages by route
const AdminDashboard = lazy(() => import("./pages/admin/dashboard"));
const ReviewPage = lazy(() => import("./pages/review"));

// In router:
<Suspense fallback={<LoadingSpinner />}>
  <Route path="/admin" component={AdminDashboard} />
  <Route path="/review" component={ReviewPage} />
</Suspense>
```

### Step 5: Optimize List Rendering

```tsx
// ✅ Always use stable keys (never array index for dynamic lists)
{items.map(item => (
  <ItemCard key={item.id} item={item} />
))}

// ✅ Virtualize long lists (50+ items)
import { useVirtualizer } from "@tanstack/react-virtual";
```

### Step 6: Reduce Bundle Size

```tsx
// ✅ Import individual Lucide icons
import { Check, X, Loader2 } from "lucide-react";

// ❌ Don't import entire icon library
import * as Icons from "lucide-react";

// ✅ Import specific Recharts components
import { BarChart, Bar, XAxis, YAxis } from "recharts";
```

## Performance Checklist

- [ ] No waterfall API calls (use Promise.all or parallel queries)
- [ ] Expensive computations wrapped in useMemo
- [ ] Callback props wrapped in useCallback
- [ ] Heavy routes use lazy loading
- [ ] Lists use stable keys (not array index)
- [ ] Individual icon imports (not barrel imports)
- [ ] Profile before optimizing (React DevTools)

## References

- `client/src/pages/review.tsx` — Complex page with memoized data
- `client/src/main.tsx` — Route setup and lazy loading
- Vercel React Best Practices (global skill: `react-best-practices`)
