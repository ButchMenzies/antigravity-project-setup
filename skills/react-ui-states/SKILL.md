---
name: react-ui-states
description: Patterns for handling loading, error, and empty states in React components with toast feedback and skeleton patterns.
---

# React UI State Patterns

Consistent patterns for loading indicators, error handling, empty states, and user feedback across the app.

## Use this skill when

- Building a page that fetches data from an API
- Handling form submissions with async operations
- Displaying lists or collections that may be empty
- Showing user feedback after actions (success/error)

## Do not use this skill when

- Building static pages with no data fetching
- Working purely on backend logic

## Instructions

### Step 1: Data Fetching State Pattern

Always handle states in this order: error → loading → empty → data.

```tsx
const { data, isLoading, error } = useQuery<MyType>({
  queryKey: ["/api/my-endpoint"],
  enabled: !!user,
});

// 1. Error
if (error) {
  return (
    <div className="flex flex-col items-center justify-center h-64 text-muted-foreground">
      <AlertCircle className="w-10 h-10 opacity-30 mb-2" />
      <p className="text-sm">Failed to load data</p>
      <p className="text-xs opacity-70">Please try refreshing the page</p>
    </div>
  );
}

// 2. Loading (only when no data exists)
if (isLoading && !data) {
  return (
    <div className="flex items-center justify-center h-64">
      <Loader2 className="w-6 h-6 animate-spin text-muted-foreground" />
    </div>
  );
}

// 3. Empty
if (!data?.items?.length) {
  return (
    <div className="flex flex-col items-center justify-center h-64 text-muted-foreground">
      <FolderOpen className="w-10 h-10 opacity-30 mb-2" />
      <p className="text-sm">No items yet</p>
      <p className="text-xs opacity-70">Create your first item to get started</p>
      <Button variant="outline" className="mt-4" onClick={handleCreate}>
        Create Item
      </Button>
    </div>
  );
}

// 4. Data
return <ItemList items={data.items} />;
```

### Step 2: Toast Notifications

Use the `useToast` hook for action feedback:

```tsx
import { useToast } from "@/hooks/use-toast";

const { toast } = useToast();

// Success
toast({
  title: "Changes saved",
  description: "Your settings have been updated",
});

// Error
toast({
  title: "Update failed",
  description: "Could not save changes. Please try again.",
  variant: "destructive",
});
```

### Step 3: Button Loading States

Always disable buttons during async operations:

```tsx
const [saving, setSaving] = useState(false);

const handleSave = async () => {
  setSaving(true);
  try {
    await apiRequest("PATCH", "/api/resource", data);
    toast({ title: "Saved" });
  } catch (error) {
    toast({ title: "Failed", variant: "destructive" });
  } finally {
    setSaving(false);
  }
};

<Button onClick={handleSave} disabled={saving}>
  {saving ? <Loader2 className="w-4 h-4 animate-spin mr-2" /> : null}
  Save Changes
</Button>
```

### Step 4: Empty State Design

Empty states should always include:
1. An icon (muted, 30% opacity)
2. A primary message
3. A secondary hint
4. An action button (when applicable)

```tsx
<div className="flex flex-col items-center justify-center h-64 text-muted-foreground gap-2">
  <Calendar className="w-10 h-10 opacity-30" />
  <p className="text-sm">No sessions this week</p>
  <p className="text-xs opacity-70">Schedule a training session to get started</p>
</div>
```

### Step 5: Conditional Content with fallback values

When data may not exist, use dash fallbacks:

```tsx
<MetricCard
  title="Goal Progress"
  value={goalProgress !== null ? `${goalProgress}%` : "—"}
  subtitle={goalProgress !== null ? `${count} of ${total}` : "No data"}
/>
```

## Checklist

- [ ] Error state shown to user (not swallowed silently)
- [ ] Loading shown only when no data
- [ ] Empty state for all lists/collections
- [ ] Buttons disabled during async operations
- [ ] Toasts for both success and error
- [ ] Fallback "—" for missing metric values

## References

- `client/src/pages/review.tsx` — Full state handling example
- `client/src/hooks/use-toast.ts` — Toast hook
- `client/src/components/MetricCard.tsx` — Metric with fallbacks
