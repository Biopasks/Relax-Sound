# AGENTS.md — AI Assistant Guide

This file provides context for AI coding assistants (opencode, Cursor, Windsurf, etc.)
to understand this project — structure, conventions, workflows, and key decisions made.

## Project Overview

- **Name**: <!-- Project Name -->
- **Repository**: <!-- https://github.com/user/repo -->
- **Production**: <!-- https://example.com -->
- **Stack**: <!-- e.g. React 19 + TypeScript 5.5 (strict) + Vite 6 + Tailwind CSS 4 -->
- **Routing**: <!-- e.g. React Router 7 (client-side, lazy routes) -->
- **State**: <!-- e.g. React Context + localStorage / Zustand / Redux -->
- **Package Manager**: <!-- npm / pnpm / yarn -->
- **Build**: <!-- `npm run build` → outputs to `dist/` -->
- **Dev**: <!-- `npm run dev` → http://localhost:5173 -->
- **Lint**: <!-- `npm run lint` -->
- **Tests**: <!-- `npm test` / `npx vitest` / `pytest` -->

## Project Structure

```
<!-- project-name/ -->
<!-- ├── src/ -->
<!-- │   ├── components/ -->
<!-- │   ├── pages/ -->
<!-- │   ├── hooks/ -->
<!-- │   ├── lib/ -->
<!-- │   └── ... -->
<!-- ├── public/ -->
<!-- ├── tests/ -->
<!-- └── ... -->
```

## Conventions

### Components
- <!-- e.g. Functional components with hooks, PascalCase filenames, one per file -->
- <!-- e.g. Props typed with interface ComponentNameProps -->
- <!-- e.g. Named exports -->

### Imports (ordered)
1. <!-- Framework / runtime (React, Vue, etc.) -->
2. <!-- Third-party libraries -->
3. <!-- Internal aliases (@/, ~/, etc.) -->
4. <!-- Styles / assets -->

### Styling
- <!-- e.g. Tailwind utility classes; cn() helper for composition -->
- <!-- e.g. Mobile-first responsive design -->
- <!-- e.g. CSS variables for theming -->

### State Management
- <!-- e.g. React Context for cross-cutting concerns -->
- <!-- e.g. localStorage for persistence on web -->
- <!-- e.g. No external state library -->

## Commands

| <!-- Command --> | <!-- Description --> |
|---|---|
| <!-- `npm run dev` --> | <!-- Start dev server (HMR) --> |
| <!-- `npm run build` --> | <!-- Production build --> |
| <!-- `npm run lint` --> | <!-- Lint code --> |
| <!-- `npm test` --> | <!-- Run tests --> |
| <!-- `npx tsc --noEmit` --> | <!-- TypeScript check --> |

## Important Files

| File | Purpose |
|------|---------|
| <!-- `src/App.tsx` --> | <!-- Root component with routes --> |
| <!-- `src/main.tsx` --> | <!-- Entry point --> |
| <!-- ... --> | <!-- ... --> |

## Known Patterns

### Adding a New Component
1. <!-- Create file in appropriate directory -->
2. <!-- Define props interface -->
3. <!-- Export and import via alias -->

### Adding a New Page / Route
1. <!-- Create file -->
2. <!-- Import and add route -->

### Adding a New API Endpoint
1. <!-- Create handler -->
2. <!-- Register route -->

## How to Use This File with AI Assistants

1. **Commit to repo** — Keep `AGENTS.md` in the project root
2. **Reference it in your prompt** — When starting a new AI session, tell the assistant:
   ```
   Read AGENTS.md for project context before making any changes.
   ```
3. **Keep it updated** — Update this file whenever you:
   - Change the tech stack or structure
   - Make key architectural decisions
   - Change build/deploy commands
   - Add new patterns or conventions

## GitHub LLM Setup

To make this file available to AI assistants via GitHub:

1. **Push to GitHub** — `git push origin main`
2. **Use raw URL** — `https://raw.githubusercontent.com/<user>/<repo>/main/AGENTS.md`
3. **Pin in repo** — Pin AGENTS.md in the repo sidebar for visibility
4. **Or reference locally** — Tools like opencode, Cursor, and Windsurf can read it
   directly from the filesystem if the repo is cloned

This file is intentionally plain markdown so any AI tool can parse it.
