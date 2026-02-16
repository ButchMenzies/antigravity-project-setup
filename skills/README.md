# Skills Library

Curated, tested skills from real Antigravity projects. When bootstrapping a new project, the agent checks here first before falling back to the [reference library](https://github.com/sickn33/antigravity-awesome-skills) or the [Superpowers library](https://github.com/obra/superpowers).

## How Skills Get Here

1. A skill is created in a project via `/create-skill`
2. The workflow automatically syncs it to this directory (Step 8a)
3. If authenticated, it's also pushed to GitHub (Step 8b)
4. Future projects can copy skills directly instead of re-creating them

## Available Skills

### Development Workflow
| Skill | Description |
|-------|-------------|
| `planning` | Principles for iterative, grounded planning — plan what you can see, learn first, then re-plan |
| `browser-testing` | Standardized browser verification workflow for testing UI changes using the browser subagent |
| `voice-notes-triage` | Analyse voice transcription notes from testing sessions to extract bugs, tasks, and improvements |

### React & Frontend
| Skill | Description |
|-------|-------------|
| `react-performance` | React performance optimization — memoization, code splitting, re-render prevention, waterfall elimination |
| `react-shadcn-dashboard` | Patterns for building admin/config dashboard pages with shadcn/ui, metric cards, and data grids |
| `react-ui-states` | Patterns for handling loading, error, and empty states with toast feedback and skeleton patterns |
| `component-accessibility` | WCAG accessibility checklist for shadcn/ui components — keyboard nav, ARIA, contrast, focus management |
| `tailwind-design-tokens` | Project-specific Tailwind design tokens — gradients, borders, hover effects, spacing, typography |

### Backend & Data
| Skill | Description |
|-------|-------------|
| `supabase` | Supabase patterns — auth, RLS, table conventions, and client initialisation (Next.js) |
| `supabase-auth-vite` | Supabase authentication patterns for Vite + Express apps — AuthContext, session flow, protected routes |
| `drizzle-supabase-patterns` | Drizzle ORM schema, migration, and data-fetching patterns for Supabase + Express backend |
| `stripe` | Stripe integration — checkout, webhooks, subscription lifecycle, and tier mapping |

### Content & Copy
| Skill | Description |
|-------|-------------|
| `copywriting` | Grammar, tone, and punctuation conventions for user-facing text |

## Structure

Each skill follows this structure:

```
skills/<skill-name>/
└── SKILL.md          # The skill definition
```

Skills may optionally include additional files (scripts, examples, templates, data) alongside SKILL.md.
