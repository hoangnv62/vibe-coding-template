# Project Structure

> Folder names vary by framework — see `tech-stack.md` for your actual choices.  
> The **component architecture principles** below apply regardless of framework.

---

## Component Architecture (The Pattern)

```
Page / Route
  ↓
[ Feature Module ]     — Self-contained: hooks + components + state + types
  ↓
[ Shared Components ]  — Reusable UI, no business logic
  ↓
[ API Layer ]          — All HTTP calls, no UI logic
  ↓
[ State Layer ]        — Global state, no UI logic
```

### Layer Responsibilities

| Layer | Owns | Must NOT |
|-------|------|---------|
| **Pages / Routes** | Layout, data fetching (SSR/SSG), routing | Business logic |
| **Feature Modules** | Feature-specific components, hooks, state | Know other features' internals |
| **Shared Components** | Reusable UI primitives | Make API calls directly |
| **API Layer** | HTTP requests, response mapping | Import UI components |
| **State Layer** | Global client state | Trigger side effects directly |

---

## Folder Mapping by Framework

> Choose your framework in `tech-stack.md`, then use the corresponding structure.

### Next.js (App Router)
```
src/
├── app/                    ← Routing (Next.js convention)
│   ├── (auth)/             ← Route groups
│   ├── (dashboard)/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── error.tsx           ← Error boundary
│   └── not-found.tsx
├── components/             ← Reusable, no business logic
│   ├── ui/                 ← Primitive components
│   ├── layout/             ← Header, Sidebar, Footer
│   └── common/             ← Shared utilities (Skeleton, ErrorState)
├── features/               ← Feature modules (co-located)
│   └── [feature]/
│       ├── components/
│       ├── hooks/
│       ├── stores/
│       └── index.ts        ← Barrel export
├── api/                    ← All HTTP calls
│   ├── client.ts           ← Axios/fetch setup
│   └── endpoints/          ← Per-resource functions
├── stores/                 ← Global state (cross-feature only)
├── hooks/                  ← Global custom hooks
├── lib/                    ← Pure utilities, constants, Zod schemas
└── types/                  ← Global TypeScript types
```

### React + Vite (SPA)
```
src/
├── pages/                  ← Route-level components (react-router)
├── components/             ← Reusable, no business logic
│   ├── ui/
│   └── layout/
├── features/               ← Feature modules (co-located)
│   └── [feature]/
│       ├── components/
│       ├── hooks/
│       ├── stores/
│       └── index.ts
├── api/                    ← All HTTP calls
├── stores/                 ← Global state
├── hooks/                  ← Global custom hooks
├── lib/                    ← Utilities
└── types/                  ← TypeScript types
```

### Remix
```
app/
├── routes/                 ← File-based routing (loader + action + component)
│   ├── _index.tsx
│   └── dashboard.tsx
├── components/
├── features/
├── api/                    ← Shared fetch helpers (called from loaders)
└── lib/
```

### SvelteKit / Nuxt / Other
> Same architectural principles apply — adapt folder names to your framework's conventions.  
> The key invariants: feature co-location, API layer isolation, pure utility lib/.

---

## Universal Conventions

Regardless of framework:

```
tests/
├── unit/           ← Pure functions, hooks (no rendering)
├── integration/    ← Component + interaction tests
└── e2e/            ← Full user flows (Playwright, Cypress)

public/             ← Static assets (favicon, robots.txt)
```

### File Naming
- **Components**: `PascalCase.tsx` — `TaskList.tsx`, `LoginForm.tsx`
- **Hooks**: `camelCase.ts` prefixed with `use` — `useTasks.ts`, `useDebounce.ts`
- **Stores**: `camelCase.ts` — `auth.store.ts`, `cart.store.ts`
- **Utilities / API**: `camelCase.ts` — `utils.ts`, `users.api.ts`
- **Test files**: `[name].test.tsx` (unit/integration), `[name].spec.ts` (E2E)

### Environment Variables
```
.env.local        ← Local dev (gitignored)
.env.example      ← Template committed to git
.env.test         ← Test env (gitignored)
.env.production   ← Set in hosting platform, never committed
```

---

## What Goes Where — Quick Reference

| If it... | Put it in... |
|----------|-------------|
| Is a page/route | `app/` or `pages/` |
| Makes HTTP calls | `api/endpoints/` |
| Is only used in one feature | `features/[name]/` |
| Is reused across features | `components/` or `hooks/` |
| Is cross-feature global state | `stores/` |
| Is feature-scoped state | `features/[name]/stores/` |
| Is a pure utility / helper | `lib/` |
| Contains TypeScript types | `types/` or `features/[name]/types/` |
| Is a static file (image, font) | `public/` or `assets/` |
