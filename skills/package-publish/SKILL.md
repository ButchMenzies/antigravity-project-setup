---
name: package-publish
description: >-
  Handles npm package publishing including bundling, exports, versioning, and
  changelog management. Use when publishing packages, creating releases,
  updating library versions, or managing changelogs.
source: antigravity
---

# Package & Publish

## When to Use

When the track involves publishing a package to npm, updating a library's public API, or preparing a release. Use for CLI tools, shared libraries, and reusable modules.

## Plan Checklist

### Package Configuration

- [ ] `package.json` has correct `name`, `version`, `description`, `license`
- [ ] `main`, `module`, and `types` (or `exports`) fields point to correct output files
- [ ] `files` array specifies only the files needed by consumers (not source, tests, config)
- [ ] `engines` field specifies minimum Node.js version if applicable
- [ ] `bin` field configured for CLI tools

### Build & Bundle

- [ ] Build produces both CJS and ESM outputs (if targeting both)
- [ ] TypeScript declaration files (`.d.ts`) generated and included
- [ ] Tree-shaking works — consumers can import individual exports without the full bundle
- [ ] Build output is clean — no dev dependencies or test files included

### Versioning

- [ ] Follow semver: major (breaking), minor (feature), patch (fix)
- [ ] Breaking changes increment major version
- [ ] CHANGELOG.md updated with changes since last release
- [ ] Git tag created matching the version number

### Pre-Publish Checks

- [ ] `npm pack --dry-run` shows only intended files
- [ ] Package installs and works correctly in a clean project (test with `npm link` or local install)
- [ ] README.md is up to date with current API, installation, and usage examples
- [ ] All tests pass

### Publish

- [ ] Authenticated with npm (`npm whoami`)
- [ ] Publish with appropriate tag: `latest` for stable, `next` for pre-releases
- [ ] Verify package appears correctly on npmjs.com after publishing

### Verification

- [ ] `npm install <package>` works in a fresh project
- [ ] Imports resolve correctly (both CJS and ESM if supported)
- [ ] TypeScript types are picked up automatically
- [ ] CLI commands work (if applicable)

## Common Mistakes

1. **Publishing dev files** — no `files` field means everything ships, including tests and config
2. **Breaking changes without major version bump** — consumers' builds break unexpectedly
3. **No type declarations** — TypeScript users get no autocomplete or type checking
4. **Forgetting to build before publish** — publishing stale output from a previous version
5. **No dry run** — publishing accidentally includes sensitive files (`.env`, secrets)
