---
name: Build Page
description: Section-by-section page construction with SEO and responsive design
triggers: [new page, landing page, marketing page, content page, build page]
track_types: [feature]
mcps: []
---

# Build Page

## When to Use

When the track involves creating a full page — landing pages, marketing pages, content pages, or any route that renders a complete view. Use alongside **build-component** for individual sections.

## Plan Checklist

### Structure

- [ ] Define page sections before building (hero, features, testimonials, CTA, footer, etc.)
- [ ] Use semantic HTML: `<header>`, `<main>`, `<section>`, `<article>`, `<footer>`
- [ ] Single `<h1>` per page — proper heading hierarchy (h1 → h2 → h3)
- [ ] Each section gets a descriptive `id` for anchor linking

### SEO

- [ ] Page title with `<title>` tag (or Next.js `metadata` export)
- [ ] Meta description (compelling, 150-160 characters)
- [ ] Open Graph tags (og:title, og:description, og:image) for social sharing
- [ ] Canonical URL if content could appear at multiple paths

### Responsive Design

- [ ] Mobile-first: design for 320px, then scale up
- [ ] Test at mobile (320px+), tablet (768px+), desktop (1024px+)
- [ ] Images use appropriate sizing: `next/image` with `sizes` prop, or responsive CSS
- [ ] No horizontal scroll at any breakpoint
- [ ] Touch targets are at least 44x44px on mobile

### Performance

- [ ] Images optimized: WebP/AVIF format, lazy loading for below-fold content
- [ ] Above-the-fold content renders without JavaScript where possible
- [ ] No layout shift — dimensions specified for images and dynamic content

### Verification

- [ ] Page renders correctly at all breakpoints
- [ ] SEO meta tags present and correct
- [ ] All links work (no 404s)
- [ ] Page is navigable by keyboard

## Common Mistakes

1. **No SEO meta tags** — pages ship without titles or descriptions
2. **Desktop-first design** — looks broken on mobile because responsive was bolted on
3. **Giant unoptimized images** — kills page load time
4. **No heading hierarchy** — multiple h1s or skipped heading levels hurt SEO and accessibility
5. **Sections without IDs** — can't link directly to page sections
