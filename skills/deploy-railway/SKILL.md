---
name: deploy-railway
description: >-
  Manages Railway deployments including service configuration, environment
  variables, and health checks. Use when deploying backends, setting up
  Railway services, or configuring production hosting.
source: antigravity
---

# Deploy to Railway

## When to Use

When the track involves deploying a backend service, API, or worker to Railway. Also use when setting up new Railway services or managing environment configuration.

## Plan Checklist

### Pre-Deploy

- [ ] Application starts cleanly: `npm start` or equivalent runs without errors
- [ ] `PORT` environment variable is used (not hardcoded) — Railway assigns ports dynamically
- [ ] Health check endpoint exists (e.g., `GET /health` returning 200)
- [ ] Build command and start command defined in `railway.toml` or Railway dashboard

### Environment Variables

- [ ] All required env vars set in Railway service settings
- [ ] Database connection string uses Railway's internal network URL (not public URL)
- [ ] Sensitive values stored as Railway variables (not in code or config files)
- [ ] Environment-specific values separated between staging and production

### Service Configuration

- [ ] Resource limits set appropriately (RAM, CPU)
- [ ] Auto-deploy enabled from the correct Git branch
- [ ] Custom domain configured (if applicable)
- [ ] Restart policy set — service recovers from crashes

### Database (if Railway-hosted)

- [ ] Database provisioned and connected
- [ ] Migrations run against the deployed database
- [ ] Backup schedule configured for production data
- [ ] Connection pooling configured for production traffic

### Verification

- [ ] Health check endpoint responds with 200
- [ ] Application logs show clean startup (no errors)
- [ ] API endpoints respond correctly
- [ ] Database connections work from the deployed service

## Common Mistakes

1. **Hardcoded PORT** — Railway assigns ports dynamically via the `PORT` env var
2. **Using public database URL** — internal network URL is faster and doesn't count against bandwidth
3. **No health check** — Railway can't detect if your service is actually healthy
4. **No migration step** — deploying code that expects new schema but migration wasn't run
5. **Missing restart policy** — service crashes and stays down instead of recovering
