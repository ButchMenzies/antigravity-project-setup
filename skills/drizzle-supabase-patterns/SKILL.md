---
name: drizzle-supabase-patterns
description: Drizzle ORM schema, migration, and data-fetching patterns for our Supabase + Express backend.
---

# Drizzle ORM + Supabase Patterns

Schema design, migration workflow, and data-fetching patterns for our Express backend using Drizzle ORM with Supabase Postgres.

## Use this skill when

- Adding new database tables or columns
- Writing queries with Drizzle ORM
- Creating API endpoints that read/write data
- Running migrations against Supabase
- Working with the `apiRequest` / React Query data layer

## Do not use this skill when

- Working on client-only UI with no data requirements
- Modifying Supabase Auth settings (use `supabase-auth-vite`)

## Instructions

### Step 1: Schema Definition

Define tables in `shared/schema.ts` using Drizzle's `pgTable`:

```typescript
import { pgTable, text, serial, integer, jsonb, timestamp, boolean } from "drizzle-orm/pg-core";
import { createInsertSchema } from "drizzle-zod";

export const myTable = pgTable("my_table", {
  id: serial("id").primaryKey(),
  userId: text("user_id").notNull(),
  name: text("name").notNull(),
  data: jsonb("data").$type<MyDataType>(),
  isActive: boolean("is_active").default(true),
  createdAt: timestamp("created_at").defaultNow(),
  updatedAt: timestamp("updated_at").defaultNow(),
});

export const insertMyTableSchema = createInsertSchema(myTable);
export type MyTable = typeof myTable.$inferSelect;
export type InsertMyTable = typeof myTable.$inferInsert;
```

**Key conventions:**
- Use `text("user_id")` for Supabase Auth user IDs
- Use `jsonb` for flexible nested data structures
- Always include `createdAt` and `updatedAt`
- Export both select and insert types
- Use `createInsertSchema` for Zod validation

### Step 2: Server-Side Storage

Create a storage class in `server/storage.ts`:

```typescript
import { db } from "./db";
import { eq, and } from "drizzle-orm";
import { myTable } from "@shared/schema";

export class MyStorage {
  async getByUserId(userId: string) {
    return db.query.myTable.findMany({
      where: eq(myTable.userId, userId),
    });
  }

  async create(data: InsertMyTable) {
    const [result] = await db.insert(myTable).values(data).returning();
    return result;
  }

  async update(id: number, data: Partial<InsertMyTable>) {
    const [result] = await db
      .update(myTable)
      .set({ ...data, updatedAt: new Date() })
      .where(eq(myTable.id, id))
      .returning();
    return result;
  }

  async delete(id: number) {
    await db.delete(myTable).where(eq(myTable.id, id));
  }
}
```

### Step 3: API Routes

In `server/routes.ts`, follow this pattern:

```typescript
app.get("/api/my-resource", async (req, res) => {
  const user = await getUser(req);
  if (!user) return res.status(401).json({ message: "Unauthorized" });

  const items = await storage.myStorage.getByUserId(user.id);
  res.json(items);
});

app.post("/api/my-resource", async (req, res) => {
  const user = await getUser(req);
  if (!user) return res.status(401).json({ message: "Unauthorized" });

  const parsed = insertMyTableSchema.safeParse(req.body);
  if (!parsed.success) {
    return res.status(400).json({ message: "Invalid data" });
  }

  const result = await storage.myStorage.create({
    ...parsed.data,
    userId: user.id,
  });
  res.json(result);
});
```

### Step 4: Client-Side Data Fetching

Use React Query with `apiRequest`:

```tsx
// GET
const { data, isLoading, error } = useQuery<MyType[]>({
  queryKey: ["/api/my-resource"],
  enabled: !!user,
});

// Mutation with cache invalidation
const mutation = useMutation({
  mutationFn: async (newItem: InsertMyTable) => {
    const res = await apiRequest("POST", "/api/my-resource", newItem);
    return res.json();
  },
  onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ["/api/my-resource"] });
    toast({ title: "Created successfully" });
  },
  onError: () => {
    toast({ title: "Failed to create", variant: "destructive" });
  },
});
```

### Step 5: Migrations

Run migrations via the Supabase MCP or directly:

```bash
# Push schema changes
npx drizzle-kit push

# Generate migration SQL
npx drizzle-kit generate
```

**Migration config** is in `drizzle.config.ts`.

## Anti-Patterns

| ❌ Don't | ✅ Do |
|----------|-------|
| Raw SQL strings | Use Drizzle query builder |
| Forget `.returning()` | Always `.returning()` on insert/update |
| Skip Zod validation | Use `insertSchema.safeParse(req.body)` |
| Forget auth check | Always call `getUser(req)` first |
| Manual cache invalidation | Use `queryClient.invalidateQueries()` |

## References

- `shared/schema.ts` — All table definitions and types
- `server/storage.ts` — Storage class with all queries
- `server/routes.ts` — API endpoint patterns
- `client/src/lib/queryClient.ts` — apiRequest + React Query setup
- `drizzle.config.ts` — Migration configuration
