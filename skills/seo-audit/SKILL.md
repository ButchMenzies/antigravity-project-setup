---
name: SEO Audit
description: Search engine optimization — meta tags, structure, performance
triggers: [SEO, search engine, meta tags, google, sitemap, indexing]
track_types: [feature, chore]
mcps: []
---

# SEO Audit

## When to Use

When the track involves improving search engine visibility, adding meta tags, or building content pages that need to rank. Also useful as a periodic audit after major page changes.

## Plan Checklist

### Page-Level SEO

- [ ] Every page has a unique `<title>` tag (50-60 characters, includes primary keyword)
- [ ] Every page has a `<meta name="description">` (150-160 characters, compelling)
- [ ] Single `<h1>` per page containing the primary keyword
- [ ] Heading hierarchy is logical (h1 → h2 → h3, no skipped levels)
- [ ] URLs are clean and descriptive (`/blog/how-to-start` not `/blog/post-123`)

### Social & Sharing

- [ ] Open Graph tags: `og:title`, `og:description`, `og:image`, `og:url`
- [ ] Twitter card tags: `twitter:card`, `twitter:title`, `twitter:description`
- [ ] OG image is at least 1200x630px

### Technical SEO

- [ ] `robots.txt` allows crawling of important pages
- [ ] `sitemap.xml` generated and includes all public pages
- [ ] Canonical URLs set on pages that could have duplicate content
- [ ] 404 page exists and is helpful
- [ ] No broken internal links

### Performance (SEO impact)

- [ ] Pages load in under 3 seconds (Core Web Vitals)
- [ ] Images have `alt` text that describes the content
- [ ] No layout shift (CLS) from lazy-loaded content
- [ ] Server-rendered content for crawlers (not client-only rendering)

### Structured Data (if applicable)

- [ ] JSON-LD schema markup for content type (Article, Product, FAQ, etc.)
- [ ] Schema validates in Google's Rich Results Test

### Verification

- [ ] View page source confirms meta tags are present (not just client-rendered)
- [ ] Social share preview looks correct (use social media debugger tools)
- [ ] All pages accessible without JavaScript for crawlers

## Common Mistakes

1. **Client-rendered meta tags** — search engines may not see tags added by JavaScript
2. **Same title/description on every page** — duplicate meta hurts rankings
3. **Missing alt text on images** — missed accessibility and SEO opportunity
4. **No sitemap** — search engines have to discover pages by crawling links
5. **Blocking crawlers accidentally** — overly aggressive robots.txt or auth on public pages
