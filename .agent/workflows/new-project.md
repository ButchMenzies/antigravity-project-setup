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

1. Web app — browser-based application
2. API / backend — server-side service or REST/GraphQL API
3. Full-stack — web app with its own backend
4. Mobile app — iOS, Android, or cross-platform
5. CLI tool — command-line utility
6. Library / package — reusable module
7. Something else — describe it
```

**Wait for user response before proceeding.**

## Tech Stack (Q2)

Based on the project type, suggest appropriate stacks. Offer 2-3 sensible defaults plus "Type your own."

### Web App / Full-Stack

```
What framework do you want to use?

1. Next.js (React, SSR, recommended for most web apps)
2. Vite + React (lightweight SPA)
3. Vite + Vue
4. Vanilla HTML/CSS/JS (no framework)
5. Type your own
```

### API / Backend

```
What backend framework?

1. Express (Node.js, widely supported)
2. Fastify (Node.js, faster)
3. FastAPI (Python)
4. Flask (Python)
5. Type your own
```

### Mobile App

```
What mobile framework?

1. React Native (JavaScript/TypeScript, cross-platform)
2. Flutter (Dart, cross-platform)
3. Native iOS (Swift)
4. Native Android (Kotlin)
5. Type your own
```

### CLI Tool

```
What language?

1. Node.js (TypeScript)
2. Python
3. Go
4. Rust
5. Type your own
```

### Library / Package

```
What language and target?

1. npm package (TypeScript)
2. PyPI package (Python)
3. Go module
4. Type your own
```

**Wait for user response before proceeding.**

## Project Details (Q3)

```
Describe what the project should do in one or two sentences.

This helps me set up the right folder structure and initial files.
```

**Wait for user response before proceeding.**

## Additional Services (Q4)

```
Will this project use any of these services? (select all that apply)

1. Database (Supabase, Postgres, MongoDB, etc.)
2. Authentication (Supabase Auth, Auth0, etc.)
3. Payments (Stripe, etc.)
4. File storage
5. None of these / not sure yet
```

**Wait for user response before proceeding.**

---

## Scaffolding

Based on the answers, scaffold the project. Follow these rules:

### General Rules

- **Use the standard tooling.** Don't hand-roll folder structures when a CLI scaffolder exists. Use `npx create-*`, `pip init`, `cargo init`, etc.
- **Non-interactive mode.** Run scaffolders with flags that avoid interactive prompts (e.g., `npx -y create-next-app@latest ./ --ts --eslint --app --src-dir --no-tailwind --import-alias "@/*"`)
- **Install in current directory.** Always scaffold into `./`, not a subdirectory
- **Check the `--help` flag first.** Before running any scaffolder, run it with `--help` to see available options. Don't guess at flags
- **TypeScript by default.** Unless the user specifically chose JavaScript, use TypeScript
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

**React Native:**
```bash
npx -y @react-native-community/cli@latest init --help
npx -y @react-native-community/cli@latest init ProjectName --directory ./
```

### After Scaffolding

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

Next step:
→ Run /setup for interactive project onboarding
→ Then /new-track to plan your first feature
```

## Error Handling

- **If scaffolding fails:** Show the error, suggest an alternative framework or manual setup
- **If the user isn't sure what to build:** Skip scaffolding, just set up a minimal structure (README.md, src/ folder), and suggest they start with `/setup` and then `/new-track` to plan
- **If the user changes their mind mid-flow:** Start over from the relevant question — don't force them through the whole flow again
