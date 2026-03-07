---
name: Deploy to Vercel
description: Vercel deployment — env vars, preview deploys, production checks
triggers: [deploy, vercel, production, preview, ship, release]
track_types: [chore]
mcps:
  - name: vercel
    usage: deployment management, environment variables
    required: false
---

# Deploy to Vercel

## When to Use

When the track involves deploying to Vercel, setting up preview environments, or managing production releases. Also use when configuring environment variables for deployment.

## Plan Checklist

### Pre-Deploy

- [ ] Build succeeds locally: `npm run build` completes with no errors
- [ ] No TypeScript errors or warnings that would fail the build
- [ ] All environment variables documented and set in Vercel dashboard
- [ ] Environment variables separated: development vs preview vs production

### Environment Variables

- [ ] All required env vars listed (compare `.env.local` with Vercel settings)
- [ ] Sensitive values (API keys, secrets) set as encrypted in Vercel
- [ ] Public env vars prefixed correctly (`NEXT_PUBLIC_` for client-accessible)
- [ ] No secrets in `NEXT_PUBLIC_` variables — they're included in the JavaScript bundle

### Preview Deploys

- [ ] Preview deployments enabled for pull requests
- [ ] Preview uses separate environment (test Stripe keys, staging database, etc.)
- [ ] Preview URL shared with stakeholders for review

### Production Deploy

- [ ] Final local build check passes
- [ ] Database migrations applied to production (if any)
- [ ] Feature flags toggled appropriately
- [ ] Post-deploy smoke test: check critical paths (login, main feature, payments)

### Verification

- [ ] Production site loads without errors
- [ ] Console has no errors or warnings
- [ ] Critical flows work end-to-end
- [ ] Performance is acceptable (no significant degradation from last deploy)

## Common Mistakes

1. **Missing environment variables** — build passes locally but fails on Vercel because env vars aren't set
2. **Secrets in `NEXT_PUBLIC_`** — exposing API keys or database URLs to the browser
3. **No local build check** — pushing code that fails `npm run build`
4. **Forgot database migration** — deploying code that references new schema before the schema exists
5. **Same env vars for preview and production** — preview writes to production database
