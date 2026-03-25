---
name: api-design
description: >-
  Enforces consistent API patterns including REST conventions, error formats,
  response structures, and versioning. Use when designing new endpoints,
  restructuring an API, or reviewing API consistency.
source: antigravity
---

# API Design

## When to Use

When the track involves designing a new API surface (multiple endpoints), establishing API conventions for a project, or refactoring inconsistent API patterns. For individual endpoints, use **create-endpoint** instead.

## Plan Checklist

### Conventions

- [ ] Define (or follow existing) response envelope format:
  ```json
  { "data": {...}, "error": null }
  { "data": null, "error": { "code": "NOT_FOUND", "message": "..." } }
  ```
- [ ] Consistent HTTP methods: GET (read), POST (create), PATCH (update), DELETE (remove)
- [ ] Consistent URL patterns: plural nouns (`/api/users`, not `/api/user`), nested resources (`/api/users/:id/posts`)
- [ ] Consistent naming: camelCase for JSON fields, kebab-case for URLs

### Error Handling

- [ ] Standard error format with machine-readable code and human-readable message
- [ ] Appropriate HTTP status codes: 400 (bad input), 401 (not authenticated), 403 (not authorized), 404 (not found), 500 (server error)
- [ ] Validation errors include field-level details
- [ ] Never return raw database errors or stack traces

### Pagination & Filtering

- [ ] List endpoints support pagination (cursor-based preferred, offset acceptable)
- [ ] Consistent pagination format: `{ data: [...], nextCursor: "..." }` or `{ data: [...], page: 1, totalPages: 5 }`
- [ ] Filter and sort parameters follow a consistent pattern across endpoints

### Documentation

- [ ] Each endpoint's contract is documented: method, path, request body, response shape
- [ ] Error responses documented with their codes and meanings
- [ ] Auth requirements noted per endpoint

### Verification

- [ ] All endpoints follow the established conventions
- [ ] Error responses are consistent and informative
- [ ] Pagination works correctly for list endpoints
- [ ] Response types match TypeScript interfaces

## Common Mistakes

1. **Inconsistent response shapes** — some endpoints return `{ data }`, others return `{ result }`, others return raw arrays
2. **No error format** — each endpoint returns errors differently
3. **Over-nesting** — `/api/org/:orgId/team/:teamId/member/:memberId/task/:taskId` is too deep
4. **Verbs in URLs** — `/api/getUsers` instead of `GET /api/users`
5. **No pagination on list endpoints** — works until there are 10,000 records
