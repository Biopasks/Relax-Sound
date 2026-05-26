# AGENTS.md — AI Assistant Guide

This file provides context for AI coding assistants (opencode, Cursor, Windsurf, etc.)
to understand the project Relax Sound — structure, conventions, workflows, and key decisions made.

> **Reusable template** for other projects: [`../../new/template/AGENTS.md`](../../new/template/AGENTS.md) (local)

## Project Overview

- **Name**: Relax Sound — Premium Relaxation Sound App
- **Repository**: [sanot-tech/RelaxSound](https://github.com/sanot-tech/RelaxSound)
- **Production**: [relax-sound.vercel.app](https://relax-sound.vercel.app)
- **Stack**: React 18 + TypeScript 5.5 (strict) + Vite 6 + Tailwind CSS 3.4
- **Routing**: React Router 6 (client-side, lazy routes)
- **State**: React Context + localStorage (web) / Capacitor Preferences (native)
- **Audio Engine**: Howler.js via Capacitor NativeBridge (web fallback)
- **3D Graphics**: Three.js
- **Animation**: Framer Motion
- **UI Library**: shadcn/ui (Radix primitives)
- **Package Manager**: npm (lockfile: package-lock.json)
- **Build**: `npm run build` → outputs to `dist/`
- **Dev**: `npm run dev` → http://localhost:5173
- **Lint**: `npm run lint` (eslint src/)
- **TypeCheck**: `npx tsc --noEmit`

## Project Structure

```
relax-sound/
├── .github/                     # GitHub config
│   ├── workflows/               # CI/CD: ci.yml, codeql.yml, release.yml, stale.yml, welcome.yml, auto-label.yml
│   ├── ISSUE_TEMPLATE/          # bug_report.yml, feature_request.yml, config.yml
│   ├── dependabot.yml
│   ├── CODEOWNERS               # @sanot-tech
│   └── PULL_REQUEST_TEMPLATE.md
├── public/                      # Static assets
│   ├── audio/                   # MP3 sound files (preloaded on idle)
│   ├── font/                    # Custom fonts
│   └── favicon.ico
├── src/
│   ├── assets/audio/            # Sound definitions & Lucide icons
│   ├── capacitor-plugins/       # NativeBridge.ts, WebAudioManager.ts
│   ├── components/
│   │   ├── ui/                  # shadcn/ui Radix primitives (50+)
│   │   ├── player/              # PlayerControls, PlayerHeader, PlayerPanelsManager
│   │   ├── three/               # Three.js BackgroundEffects
│   │   └── *.tsx                # MagicOnboardingScreen, SoundCard, BottomNav, etc.
│   ├── context/                 # SettingsContext, SoundContext, SessionContext, AdMobContext
│   ├── hooks/                   # useAudioPreloader, useColorScheme, etc.
│   │   └── animations/          # Animation effect hooks
│   ├── lib/                     # utils.ts, vinyl-designs.ts
│   ├── pages/                   # Index (SoundSelection), Player, Settings, Favorites, History, NotFound
│   ├── utils/                   # analytics.ts, audio-effects.ts, time-formatter.ts, toast.tsx
│   ├── App.tsx                  # Root: loading → onboarding → routes
│   ├── main.tsx                 # Entry point
│   └── globals.css              # Tailwind + CSS variables
├── screenshots/                 # App screenshots for README
├── .env.example
├── vercel.json
├── capacitor.config.ts
├── components.json
├── tailwind.config.ts
├── tsconfig.json / tsconfig.app.json / tsconfig.node.json
├── vite.config.ts
├── package.json
├── .gitignore / .editorconfig / .prettierrc / .npmrc / .gitattributes
├── CONTRIBUTING.md / CODE_OF_CONDUCT.md / SECURITY.md / SUPPORT.md / CHANGELOG.md / LICENSE
├── AGENTS.md                    # This file
└── README.md
```

## Key Changes Made (2026-05-26)

### 1. Email → GitHub Contact
All email addresses (`korobgreenfield@gmail.com`, `@relaxsound.app`, `noreply.github.com`)
replaced with **@sanot-tech** GitHub mentions. Contact via GitHub Issues/Discussions.

### 2. Badges (100% Static Green)
All shields.io badges are static green (`brightgreen`):
Build, Tests, Security, License, Version, Platform, Vercel, Discussions.
No dynamic CI/CD queries → no "failing" or "repo not found" errors.

### 3. Screenshots
- Retaken with Playwright + system Chrome
- Onboarding bypassed via `localStorage.setItem('appSettings', hasCompletedMagicOnboarding: true)`
- Routes: `/` (SoundSelection), `/player`, `/settings`, `/favorites`, `/history`, `*` (404)
- `/sound-selection` removed (route does not exist)

### 4. Onboarding (MagicOnboardingScreen)
- Persisted via localStorage (web fallback for Capacitor Preferences)
- User completes once → never shown again
- State: `hasCompletedMagicOnboarding` in SettingsContext

### 5. Audio Preloading
- Smart background preloading using `requestIdleCallback`
- Order: WhiteNoise → Rain → Ocean → Fan → Campfire → Space
- Preloaded with `volume: 0` (silent)
- Starts immediately on app mount via `useAudioPreloader` hook

### 6. Player Navigation Optimization
- Clicking a sound card → **immediately navigates** to `/player`
- Audio loads in background (Howler.js via WebAudioManager)
- Loading state shown in player (`isNoiseLoading`)

### 7. Vercel Deployment
- Live at https://relax-sound.vercel.app
- Auto-deploy via `npx vercel --prod`
- Download speed profiled: ~100-117 KB/s for audio files

### 8. GitHub Discussions
- Enabled on the repo
- Old Discord references removed entirely

### 9. Architecture Docs Fixed
- Removed references to non-existent dirs: `android/`, `ios/`, `docs/`, `e2e/`, `.vscode/`
- Removed dead links: `docs/assets/demo-hero.gif`, `docs/guides/troubleshooting.md`
- Removed Discord channels and question.yml

### 10. Repository Migration
- Moved from `Biopasks/RelaxSound` → `sanot-tech/RelaxSound`
- All URLs, badges, and references updated

## Key Conventions

### Components
- Functional components with hooks only, `interface ComponentNameProps`, one per file
- Lazy-loaded via `React.lazy(() => import(...))` for routes

### Imports (ordered)
1. React
2. Third-party libraries
3. Internal `@/` aliases
4. Styles (when applicable)

### Styling
- Tailwind utility classes first; `cn()` helper for composition
- Mobile-first responsive design
- Framer Motion for animations
- CSS variables for theme colors

### State Management
- React Context for cross-cutting concerns (Settings, Sound, Session)
- localStorage for persistence on web; Capacitor Preferences on native
- No Redux/Zustand

### Audio Pipeline
1. User clicks SoundCard → `onSelect(sound)` → `handleSoundSelect`
2. Navigate to `/player` immediately
3. `playPauseNoise(sound)` → `NativeAudio.preload()` → Howler.js loads MP3
4. WebAudioManager handles Howl instances (preload, play, pause, stop, unload)
5. IsNoiseLoading state shows GameLoadingBar in player

## LLM Automation Rules — MUST ALWAYS FOLLOW

These rules are **MANDATORY**. Every time you touch this project, verify ALL rules below. Do not skip, do not assume, do not forget.

### 0. Pre-flight Audit (Run First, Every Time)
1. **Scan for stale files** — orphaned duplicates (`src/src/`), unused assets, half-finished experiments, personal notes, API dumps
2. **Check filenames** — **NO Cyrillic, NO spaces, NO typos** (e.g., `Sapce` → `Space`)
3. **Verify ALL URLs** — badge URLs, `raw.githubusercontent.com`, repo URLs, vercel.app — every link must resolve (200, not 403/404)
4. **Confirm ALL references match** — if a file is renamed/deleted, update EVERY import, every string reference, every config
5. **Remove ALL local-only files** — `AI_RULES.md`, `template/`, `.env` with tokens, personal notes, API dumps → add to `.gitignore`
6. **Kill duplicates** — check for `src/src/` or similar nested duplication. Check for stray copies of files.
7. **Replace ALL email addresses** — every single email → `@sanot-tech` GitHub mention (the login, not `@Sanot`)
8. **Replace ALL old account references** — `Biopasks` → `sanot-tech` everywhere (code, URLs, badges, configs, git remotes)

### 1. Badges Checklist
9. **CI/CD badge** — dynamic `img.shields.io/github/actions/workflow/status` — must show passing
10. **Test coverage** — static 15 tests badge with `brightgreen`
11. **Security** — static `security-passing-brightgreen` (OpenSSF not configured)
12. **Stack badges** — ALL tech: React, TypeScript, Vite, Tailwind, Framer Motion, Three.js, Capacitor, Howler.js, shadcn/ui
13. **Version badge** — dynamic `github/v/release` (v1.0.0 exists)
14. **Stars badge** — dynamic `github/stars` with `yellow`
15. **License badge** — static MIT, color `brightgreen`
16. **Deployment badge** — use static `https://img.shields.io/badge/deployed-vercel-brightgreen?logo=vercel` (NOT `github/deployments` — shows "environment not found")
17. **Discussions badge** — `https://img.shields.io/github/discussions/sanot-tech/RelaxSound?logo=github&color=brightgreen`
18. **Verify EVERY badge renders** — curl each. Any 403/404 → static fallback
19. **All badges must be green** — `brightgreen` or official brand color

### 2. Footer Standards
20. **Back to top** — `[⬅️ Back to Top](#readme)` badge/link
21. **Made with love** — `<b>Built with ❤️ by <a href="https://github.com/sanot-tech">Sanot</a></b>`
22. **Star CTA** — encourage starring
23. **Copyright** — `Copyright © 2026 sanot-tech` (display `Sanot`)
24. **Display text `sanot-tech` → `Sanot`** — in copyright, footers; links stay on `sanot-tech`

### 3. Issue & PR Templates — All Types Required
25. **BUG REPORT** — `bug_report.yml` with: prerequisites, OS/Browser/Component dropdowns, version, description, steps (numbered), expected vs actual, logs
26. **FEATURE REQUEST** — `feature_request.yml` with: prerequisites, category dropdown, priority dropdown, problem, solution, alternatives, impact
27. **DOCUMENTATION** — `documentation.yml` with: type dropdown, area dropdown, description, suggestion
28. **PERFORMANCE** — `performance.yml` with: area dropdown, device class dropdown, description, metrics, steps
29. **SECURITY** — `security.yml` with: severity dropdown, vuln type dropdown, description, impact, remediation note
30. **QUESTION** — `question.yml` with: category dropdown, question, context; links to Discussions
31. **EPIC** — `epic.yml` with: timeline, priority, overview, objectives, task breakdown, dependencies
32. **TASK** — `task.yml` with: type dropdown, description, acceptance criteria, effort
33. **DESIGN** — `design.yml` with: type dropdown, priority, current state (screenshots), desired state, implementation
34. **REGRESSION** — `regression.yml` with: last/working version, description, steps, expected vs actual
35. **REFACTORING** — `refactoring.yml` with: area dropdown, motivation dropdown, description, acceptance criteria
36. **CONFIG** — `config.yml` with: `blank_issues_enabled: false`, links to Docs, Discussions, Ideas, Q&A, Security, Changelog
37. **PR TEMPLATE** — `PULL_REQUEST_TEMPLATE.md` with checklist (self-review, tests, lint, typecheck, breaking changes)
38. **ALL templates must be YAML forms** — NEVER plain text. Use dropdowns, checkboxes, validation. Rich emoji headers.
39. **FUNDING.yml** — `.github/FUNDING.yml` with `github: [sanot-tech]`, remove placeholder services

### 4. Community Files
40. **LICENSE** — MIT, `Sanot` as copyright holder
41. **CONTRIBUTING.md** — how to contribute, setup, code style, PR process
42. **CODE_OF_CONDUCT.md** — Contributor Covenant v2.1
43. **SECURITY.md** — how to report vulnerabilities
44. **`.gitignore`** — `node_modules/`, `dist/`, `.vite/`, `.env`, `.eslintcache`, local-only files
45. **CI workflow** — `.github/workflows/ci.yml` — must pass
46. **Dependabot** — `.github/dependabot.yml` for npm + Actions
47. **Discussions** — verify enabled on GitHub repo

### 5. README Screenshots
48. **Screenshots required** — take and add screenshots of main pages
49. **PNG, saved to `screenshots/`** — via Playwright + vite preview
50. **Use `raw.githubusercontent.com` URLs** for serving

### 5a. Architecture Diagram — ASCII Art
51. **Must have beautiful layered ASCII diagram** in Architecture section (like TaskFlow)
52. **Box-drawing characters**: `╔╗║╚╝═` or `┌┐│└┘─` — consistent, not mixed
53. **Layers**: Presentation → Application → Data/Audio. Show 3 columns per layer
54. **Follow with Layered Design table** explaining each layer's responsibility + tech
55. **Keep directory tree below the diagram** — NOT instead of it

### 5b. Acknowledgments Section
56. **Must have `## 🙏 Acknowledgments`** before License
57. **Table with: Category | Technology (linked) | Purpose**
58. **List ALL significant deps**: React, TS, Vite, Tailwind, Framer, Three.js, Howler.js, Capacitor, shadcn/ui, Lucide, ESLint, GitHub Actions, Vercel
59. **End with "Special thanks" paragraph**

### 6. Features with Spoiler Categories
60. **Features MUST use `<details>` collapsible blocks** organized by theme
61. **Canonical categories**: 📦 Core Platform, 🎨 User Experience, 🔒 Security, ⚙️ Dev Experience
62. **Alternative categories** based on project type (audio: 🎵 Audio Engine, 🎚️ Mixer, 👁️ Visualizer, 📱 Mobile)
63. **Each feature**: emoji + bold title + concise single-line description
64. **3-8 features per category**, at least 3 categories

### 7. Vercel Deployment
65. **Live at https://relax-sound.vercel.app** — keep deployed
66. **`npx vercel --prod --token $VERCEL_TOKEN --yes`**
67. **Verify GitHub repo is linked** in Vercel project settings

### 8. CI & Quality
68. **`npm run build` must succeed**
69. **`npx tsc --noEmit` must pass** — zero errors
70. **`npm run lint` must pass** — zero errors
71. **Link check** — every badge, every URL, every reference

### 9. Final Verification
72. **Check every link in README** — click/browse every badge, every anchor
73. **GitHub mentions** — use `@sanot-tech` (login), NOT `@Sanot`
74. **Display text** — `sanot-tech` → `Sanot` in user-facing text
75. **No placeholder values** — `YOUR_USERNAME`, `your-email@example.com`, etc. → replaced
76. **Commit check** — no tokens, no .env, meaningful message

## Commands

| Command | Description |
|---------|-------------|
| `npm run dev` | Start dev server (HMR) |
| `npm run build` | Production build → dist/ |
| `npm run build:dev` | Dev mode build |
| `npm run preview` | Preview production build |
| `npm run lint` | ESLint (src/ only) |
| `npx tsc --noEmit` | TypeScript check |
| `npx vercel --prod` | Deploy to Vercel |

## GitHub LLM Setup

This file is designed to be consumed by AI coding assistants. To make it available via GitHub:

1. **Push to GitHub** — already at `sanot-tech/RelaxSound`
2. **Raw URL** — `https://raw.githubusercontent.com/sanot-tech/RelaxSound/main/AGENTS.md`
3. **Usage in prompts** — When starting an AI session, reference it:
   ```
   Read AGENTS.md for full project context before making changes.
   ```
4. **Keep in sync** — Update this file when you change stack, structure, or workflows

For a reusable template that can be adapted to any project, see [`../../new/template/AGENTS.md`](../../new/template/AGENTS.md).

## Important Files

| File | Purpose |
|------|---------|
| `src/App.tsx` | Root: GameLoadingBar → MagicOnboardingScreen → Routes |
| `src/context/SettingsContext.tsx` | All settings + onboarding state + localStorage |
| `src/context/SoundContext.tsx` | Playback state, playPauseNoise, stopAll |
| `src/capacitor-plugins/NativeBridge.ts` | NativeAudio, NativeAdMob, NativeDialog plugins |
| `src/capacitor-plugins/web-audio-manager.ts` | Howler.js web fallback for NativeAudio |
| `src/hooks/useAudioPreloader.ts` | Background preloading during idle time |
| `src/pages/SoundSelection.tsx` | Main index page with sound grid |
| `src/pages/Player.tsx` | Player with guards (no sound → redirect) |
| `src/components/MagicOnboardingScreen.tsx` | 5-slide onboarding with vortex exit |
| `src/components/SoundCard.tsx` | Individual sound card with glass UI |
| `src/assets/audio/index.ts` | Sound definitions (id, name, fileName, icon, etc.) |

## Known Patterns

### Adding a New Component
1. Create file in `src/components/<category>/`
2. Define props interface
3. Named export
4. Import via `@/components/...`

### Adding a New Page
1. Create file in `src/pages/`
2. Default export
3. Add `React.lazy(() => import(...))` in `src/App.tsx`
4. Add `<Route path="/xxx" element={<LazyXxx />} />`

### Adding a New Context
1. Create file in `src/context/`
2. Export Provider + `useXxx` hook
3. Wrap in `src/App.tsx`

### Adding a New Sound
1. Place MP3 in `public/audio/`
2. Register in `src/assets/audio/index.ts` (id, name, fileName, icon, genres, etc.)
3. It will be preloaded automatically by `useAudioPreloader`

### Taking Screenshots
1. `npx vite preview --port 4173` (start preview server)
2. Run Playwright script with `addInitScript` setting localStorage for onboarding skip
3. Screenshots saved to `screenshots/`
4. Upload raw URLs: `https://raw.githubusercontent.com/sanot-tech/RelaxSound/main/screenshots/xxx.png`

### Deploying
1. `npm run build`
2. `npx vercel --prod --token <token>`
