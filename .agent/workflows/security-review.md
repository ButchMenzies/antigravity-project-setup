---
version: 1
description: Comprehensive security review — database, API, auth, secrets, infrastructure, client-side
---

# Security Review

Run this workflow after major phases, before deployment, or whenever security needs checking. This is a standalone review — it can be triggered at any time and adapts to your tech stack.

> **Every review builds on the last.** Results are stored in `conductor/product.md` under the Security section, creating a living security record.

---

## Phase 1: Context

Read the current security posture before doing anything new.

1. **Read previous findings** — check `conductor/product.md` for the `## Security` section
   - If it exists: note the last review date, any open warnings/critical items, and accepted risks
   - If it doesn't exist: this is the first review — note that for the report
2. **Detect the stack** — read `conductor/tech-stack.md` (or scan `package.json`, config files, deployment configs)
   - Note: database provider, auth provider, hosting/deployment platform, framework
3. **Read `.agent/memory.md`** — check for past security decisions or known issues
4. **Read `.agent/AGENT.md`** — understand what the project is and what it handles (user data, payments, etc.)

Present a brief summary:

```
🔒 Security Review — Starting

Last review: [date or "First review"]
Open items from last review: [N items or "None"]

Detected stack:
- Framework: [Next.js / Vite / Express / etc.]
- Database: [Supabase / Prisma+Postgres / PlanetScale / none / etc.]
- Auth: [Supabase Auth / NextAuth / Clerk / custom / none / etc.]
- Hosting: [Vercel / Railway / Netlify / Cloudflare / self-hosted / etc.]
- Edge Functions: [yes/no]

Proceeding with checks tailored to this stack.
```

---

## Phase 2: Database & Data Layer

Skip this phase if the project has no database.

### Universal checks

- **Schema review** — are sensitive fields (passwords, tokens, PII) stored appropriately?
- **Migration files** — do any migrations contain hardcoded secrets or test data?
- **Query patterns** — scan for string interpolation in SQL (SQL injection risk). Look for raw queries, template literals with user input, or string concatenation in query builders.

### Supabase (if detected)

- Run `list_tables` to get current schema
- Verify **RLS is enabled** on ALL public tables
- Check that **RLS policies exist** for every table (authenticated + anon as appropriate)
- Run `get_advisors` with type `"security"` to check for known issues
- Run `get_advisors` with type `"performance"` — performance issues sometimes surface security-adjacent problems
- Review tables with sensitive data for overly permissive policies
- Check Storage buckets — public vs private classification, RLS policies on `storage.objects`

### Prisma / Drizzle / TypeORM (if detected)

- Review schema for missing `@unique` constraints on auth-related fields
- Check for raw query usage (`$queryRaw`, `sql`, etc.) — verify parameterisation
- Review middleware/hooks for access control enforcement

### Railway / PlanetScale / other managed DB (if detected)

- Check connection strings aren't hardcoded
- Verify SSL/TLS is enforced for database connections
- Check for public network exposure settings

---

## Phase 3: API & Route Security

Adapt route scanning to the framework.

### Find routes

| Framework | Where to look |
|-----------|---------------|
| Next.js (App Router) | `app/**/route.ts`, `app/api/**` |
| Next.js (Pages) | `pages/api/**` |
| Express / Fastify | Route definitions, `app.get/post/etc.` |
| SvelteKit | `src/routes/**/+server.ts` |
| Astro | `src/pages/api/**` |
| Nuxt | `server/api/**` |

### For each route, check

1. **Authentication** — is there a session/token check before processing? Look for unprotected routes that should require auth.
2. **Input validation** — are POST/PATCH/PUT bodies validated? Look for missing schema validation (zod, joi, yup, etc.).
3. **Error handling** — are internal errors caught and sanitised? Look for:
   - Stack traces leaked in responses
   - Database error messages exposed to clients
   - Generic `catch(e) { res.json(e) }` patterns
4. **HTTP methods** — are routes restricted to expected methods?
5. **Rate limiting** — is there any rate limiting on auth-related or expensive endpoints?

### CORS (if applicable)

- Check for `Access-Control-Allow-Origin: *` — is it intentional?
- Review allowed methods and headers

---

## Phase 4: Authentication & Authorisation

Skip this phase if the project has no auth.

### Universal checks

- **Session management** — how are sessions handled? Secure cookie flags? Token expiry?
- **Password requirements** — if custom auth, are passwords hashed (bcrypt, argon2)?
- **Role-based access** — if roles exist, are they enforced server-side (not just client-side UI hiding)?
- **Logout** — does logout actually invalidate the session/token?

### Supabase Auth (if detected)

- Check enabled auth providers
- Review JWT expiry settings
- Verify email confirmation is enabled if the app requires verified users
- Check OAuth redirect URLs — are they restricted to known domains?
- Review any custom claims or hooks

### NextAuth / Auth.js (if detected)

- Check `NEXTAUTH_SECRET` is set and not a default value
- Review callback URLs configuration
- Check session strategy (JWT vs database)
- Review provider configurations for proper scopes

### Clerk / Auth0 / other providers (if detected)

- Check API keys are server-side only
- Review webhook verification
- Check redirect/callback URL restrictions

---

## Phase 5: Secrets & Environment

### Automated scan

Run a grep for potentially exposed secrets in source files:

```bash
grep -rn --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' --include='*.py' --include='*.go' \
  -E '(sk_live|sk_test|password\s*=\s*["\x27]|secret\s*=\s*["\x27]|token\s*=\s*["\x27]|api_key\s*=\s*["\x27]|PRIVATE_KEY|BEGIN RSA)' \
  . --exclude-dir=node_modules --exclude-dir=.git --exclude-dir=.next --exclude-dir=dist | head -30
```

### Manual checks

1. **`.gitignore` coverage** — verify these are ALL ignored:
   - `.env`, `.env.local`, `.env.production`, `.env*.local`
   - Any other files that might contain secrets
2. **Client-side exposure** — check environment variable prefixes:
   - `NEXT_PUBLIC_*` (Next.js) — should these be public?
   - `VITE_*` (Vite) — should these be public?
   - `EXPO_PUBLIC_*` (Expo) — should these be public?
   - Rule: only anon/publishable keys and public URLs should use public prefixes
3. **Hardcoded values** — search for:
   - API keys, connection strings, or tokens directly in source files
   - Default/placeholder credentials that were never changed
   - Comments containing real credentials
4. **Git history** — if a secret was previously committed, flag it (even if removed, it's in history)

---

## Phase 6: Infrastructure & Deployment

Adapt to the detected hosting platform.

### Supabase (if detected)

- **Edge Functions** — check JWT verification is enabled where appropriate (review `verify_jwt` setting)
- **Edge Function code** — scan for exposed secrets or unsafe patterns
- **Vault** — is Vault used for sensitive configuration instead of environment variables?
- **Storage** — bucket policies, file size limits, allowed MIME types
- **Realtime** — are RLS policies applied to realtime subscriptions?

### Vercel (if detected)

- Check `vercel.json` for exposed headers or security-relevant rewrites
- Review environment variables — are preview/production properly scoped?
- Check for missing security headers (CSP, X-Frame-Options, etc.)
- Serverless function timeout settings — could long-running functions be abused?

### Railway (if detected)

- Check for private networking between services
- Review environment variable configuration
- Verify services aren't publicly exposed unnecessarily
- Check for health check endpoints that leak system info

### Netlify (if detected)

- Review `netlify.toml` for redirect rules and headers
- Check serverless function configuration
- Review form handling settings

### Cloudflare (if detected)

- Review Workers configuration
- Check for security headers in `_headers` file
- Review access policies

### General (all platforms)

- **HTTPS** — is it enforced? No mixed content?
- **Domain configuration** — any dangling DNS records?
- **Build output** — is the `.next/`, `dist/`, or build directory gitignored?

---

## Phase 7: Client-Side Security

### XSS vectors

- Search for `dangerouslySetInnerHTML`, `v-html`, `innerHTML` usage
- Check that user-generated content is sanitised before rendering
- Review URL parameter handling — are query params rendered without escaping?

### Security headers

Check for these headers (in middleware, config, or hosting platform):
- `Content-Security-Policy` — restrictive CSP?
- `X-Frame-Options` — clickjacking protection?
- `X-Content-Type-Options: nosniff`
- `Strict-Transport-Security` — HSTS?
- `Referrer-Policy`

### Dependencies

```bash
npm audit 2>/dev/null || yarn audit 2>/dev/null || pnpm audit 2>/dev/null
```

Flag any high or critical vulnerabilities.

### Source maps

- Are source maps disabled in production builds?
- Check framework config (`next.config.js`, `vite.config.ts`, etc.)

---

## Phase 8: Report & Record

### Compile the report

Categorise every finding:

```
🔒 Security Review — [Date]

Stack: [Framework] + [Database] + [Auth] + [Hosting]

✅ Passed:
- [Items that are secure — be specific]

⚠️ Warnings:
- [Issues that should be addressed but aren't immediately dangerous]
- [Include file references and line numbers where applicable]

🚨 Critical:
- [Items that must be fixed before deployment]
- [Include exact location and suggested fix]

📋 Not Applicable:
- [Checks that were skipped and why]

💡 Recommendations:
- [Suggested improvements for hardening]

Compared to last review:
- [Items resolved since last time]
- [New items found]
- [Items still open]
```

**Present the report to the user before recording.**

### Update conductor/product.md

Add or update the `## Security` section:

```markdown
## Security

Last reviewed: [Date]
Status: [✅ Clean / ⚠️ Warnings / 🚨 Action Required]

### Findings
| Date | Category | Severity | Finding | Status |
|------|----------|----------|---------|--------|
| [Date] | [Category] | [✅/⚠️/🚨] | [Brief description] | [Open/Fixed/Accepted] |
```

Rules for the findings table:
- **Append** new findings — don't delete old rows
- **Update status** of previously open items (Open → Fixed, Open → Accepted)
- Keep a rolling history — this IS the security audit trail
- Remove resolved items only after 3+ reviews to maintain context

If `conductor/product.md` doesn't exist (no conductor), store the report in `.agent/memory.md` instead under a `## Security Reviews` section.

### Update memory

Log the review in `.agent/memory.md`:

```markdown
| [Date] | Security review: [one-line outcome summary] |
```

### Next steps

```
What would you like to do with these findings?

1. Fix critical items now
2. Add warnings to the roadmap
3. Add all findings to the roadmap
4. Nothing for now — I've noted everything
```

**Wait for user response.** Only modify roadmap or other documents with explicit approval.
