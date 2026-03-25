---
name: performance-audit
description: >-
  Runs framework-specific performance checks covering bundle size, rendering,
  and caching. Use when optimizing page load times, reducing bundle size,
  or auditing application performance.
source: antigravity
---

# Performance Audit

## When to Use

When the track involves improving application performance, or as a periodic check after major feature work. Most useful for web apps and websites.

## Plan Checklist

### Component Architecture (Next.js / React)

- [ ] Server vs client component split: are client components only where interactivity is needed?
- [ ] Check for `"use client"` directives — each one adds to the JavaScript bundle
- [ ] Large client components that could be partially server-rendered
- [ ] Suspense boundaries around async components for streaming

### Bundle & Loading

- [ ] Check for large dependencies — are there lighter alternatives?
- [ ] Dynamic imports (`next/dynamic`, `React.lazy`) for heavy components not needed on initial load
- [ ] Images using `next/image` (or framework equivalent) with appropriate `sizes` and formats
- [ ] Fonts loaded via `next/font` (not external CDN requests)

### Data Fetching

- [ ] No waterfall fetches — parallel where possible
- [ ] Appropriate caching: `revalidate` settings on server fetches
- [ ] No unnecessary re-fetching — data that doesn't change often should be cached
- [ ] No client-side fetching for data that could be server-rendered

### Rendering

- [ ] No layout shift — all dynamic content has reserved space
- [ ] Above-the-fold content renders without waiting for client JavaScript
- [ ] Loading and skeleton states prevent blank screens
- [ ] Lists are virtualized if they can exceed ~50 items

### Verification

- [ ] Page loads feel fast — no visible content shift or blank states
- [ ] Build output shows reasonable bundle sizes (investigate any page > 200kb JS)
- [ ] Lighthouse performance score checked (if applicable)

## Common Mistakes

1. **Everything as client components** — shipping unnecessary JavaScript to the browser
2. **Client-side fetching for static data** — data that could be fetched at build time or on the server
3. **No image optimization** — raw PNGs or uncompressed images killing load times
4. **Import entire libraries** — importing all of lodash when you need one function
5. **No caching strategy** — every page fetch hits the database on every request
