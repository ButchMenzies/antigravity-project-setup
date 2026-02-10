---
description: Scaffold a brand new project — choose what to build, create folder structure, install dependencies
---

# New Project

Scaffold a new project from a blank folder. This workflow is triggered by the setup guide when it detects an empty project directory.

## Pre-flight

The setup guide has already bootstrapped `.agent/` (workflows, skills, templates) before triggering this workflow. The project folder has no source files — only the `.agent/` directory.

1. Read `.agent/skills/planning/SKILL.md` — apply planning principles when discussing project scope

## Project Type (Q1)

```
What are you building?

1. Web app — browser-based application (or full-stack with API)
2. API / backend — server-side service or REST/GraphQL API
3. Mobile app — iOS, Android, or cross-platform
4. CLI tool — command-line utility
5. Library / package — reusable module
6. Something else — describe it
```

**Wait for user response before proceeding.**

## Description (Q2)

```
Describe what it should do in one or two sentences.

This helps me set up the right structure and initial files.
```

**Wait for user response before proceeding.**

## Tech Stack (Q3)

Based on the project type, **recommend a default stack** with a brief explanation. The user can accept or change it.

### Web App / Full-Stack

```
For a web app, I'd recommend:

→ Next.js with TypeScript (App Router)

This gives you:
- Server-side rendering and static generation
- Built-in API routes (no separate backend)
- Works well with Supabase, Stripe, and other services
- Largest ecosystem and community

Happy with this?

1. Sounds good — let's go with Next.js
2. I want something lighter (Vite + React — pure client-side SPA)
3. I want no framework (vanilla HTML/CSS/JS)
4. I have something else in mind
```

### API / Backend

```
For an API, I'd recommend:

→ Express with TypeScript

This gives you:
- Most widely supported Node.js framework
- Huge middleware ecosystem
- Easy to extend with auth, validation, etc.

Happy with this?

1. Sounds good — let's go with Express
2. I want something faster (Fastify)
3. I'd prefer Python (FastAPI)
4. I have something else in mind
```

### Mobile App

```
For a mobile app, I'd recommend:

→ React Native with Expo (TypeScript)

This gives you:
- Cross-platform (iOS + Android from one codebase)
- JavaScript ecosystem (same as web)
- Expo handles the painful native tooling

Happy with this?

1. Sounds good — let's go with React Native + Expo
2. I'd prefer Flutter (Dart)
3. I want native iOS only (Swift)
4. I want native Android only (Kotlin)
5. I have something else in mind
```

### CLI Tool

```
For a CLI tool, I'd recommend:

→ Node.js with TypeScript

This gives you:
- Fast to build, easy to distribute via npm
- Great CLI libraries (commander, inquirer)

Happy with this?

1. Sounds good — Node.js + TypeScript
2. I want a compiled binary (Go)
3. I want a compiled binary (Rust)
4. I'd prefer Python
5. I have something else in mind
```

### Library / Package

```
For a library, I'd recommend:

→ TypeScript npm package

This gives you:
- Type-safe API for consumers
- Easy publishing to npm

Happy with this?

1. Sounds good — TypeScript npm package
2. I'd prefer a Python package (PyPI)
3. I'd prefer a Go module
4. I have something else in mind
```

**Wait for user response before proceeding.**

---

## Scaffolding

Based on the answers, scaffold the project. Follow these rules:

### General Rules

- **Use the standard tooling.** Don't hand-roll folder structures when a CLI scaffolder exists. Use `npx create-*`, `npm init`, `cargo init`, etc.
- **Non-interactive mode.** Run scaffolders with flags that avoid interactive prompts
- **Check the `--help` flag first.** Before running any scaffolder, run it with `--help` to see available options. Don't guess at flags
- **Install in current directory.** Always scaffold into `./`, not a subdirectory
- **TypeScript by default.** Unless the user specifically chose JavaScript
- **Git init.** If the scaffolder doesn't create a git repo, run `git init`

### Framework-Specific Notes

**Next.js:**
```bash
npx -y create-next-app@latest ./ --help  # Check available flags first
npx -y create-next-app@latest ./ --ts --eslint --app --src-dir --import-alias "@/*"
```

**Vite + React:**
```bash
npx -y create-vite@latest ./ --help
npx -y create-vite@latest ./ --template react-ts
npm install
```

**Express:**
```bash
npm init -y
npm install express
npm install -D typescript @types/express @types/node tsx
npx tsc --init
mkdir -p src
# Create src/index.ts with basic Express server
```

**React Native + Expo:**
```bash
npx -y create-expo-app@latest ./ --help
npx -y create-expo-app@latest ./ --template blank-typescript
```

### Additional Folders

After the scaffolder runs, create these folders if they don't already exist:

```bash
mkdir -p docs/design          # UI mockups, screenshots, design exports
mkdir -p public/assets         # Logo, favicon, images (web projects)
```

For web/full-stack projects, ensure there is a dedicated styles directory (e.g., `src/styles/`) for CSS variables and design tokens.

### Verify and Commit

1. **Verify it works:**
   ```bash
   npm run dev  # or equivalent
   ```
   If the dev server starts, kill it after confirming

2. **Set up .gitignore** if it doesn't exist — use the standard template for the framework

3. **Initial commit:**
   ```bash
   git add -A
   git commit -m "Initial scaffold: [framework name]"
   ```

---

## Completion

```
✅ Project scaffolded!

Created: [framework] project with [language]
Structure: [brief description of key folders]

💡 Before building features, consider setting up your brand identity
   and core UI as your first /new-track. Tools like Google Stitch
   (stitch.withgoogle.com) can help generate UI mockups and ideas.

Next steps:
→ Run /setup for interactive project onboarding
→ Then /new-track to plan your first piece of work
```

## Error Handling

- **If scaffolding fails:** Show the error, suggest an alternative framework or manual setup
- **If the user isn't sure what to build:** Skip scaffolding, just set up a minimal structure (README.md, src/ folder), and suggest they start with `/setup` and then `/new-track` to plan
- **If the user changes their mind mid-flow:** Start over from the relevant question — don't force them through the whole flow again
