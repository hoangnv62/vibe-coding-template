# Project Structure

> Folder names vary by framework and monorepo tool — see `tech-stack.md` for your choices.  
> The **architectural principles** below apply regardless of setup.

---

## Fullstack Architecture Overview

```
Browser / Mobile Client
        ↓  HTTP / WebSocket
[ Frontend ]  →  components, routing, state, forms
        ↓  REST / GraphQL / tRPC / Server Actions
[ Backend API ]  →  routes, controllers, services, auth
        ↓
[ Domain / Business Logic ]
        ↓
[ Infrastructure ]  →  database, cache, queue, storage, email
```

---

## Monorepo Structures

### Option A: Turborepo / pnpm workspaces (Recommended for teams)
```
project-root/
├── apps/
│   ├── backend/            ← API server (Express / NestJS / FastAPI / ...)
│   │   ├── .claude/        ← backend-specific Claude config
│   │   ├── src/
│   │   └── package.json
│   └── frontend/           ← UI (Next.js / React+Vite / ...)
│       ├── .claude/        ← frontend-specific Claude config
│       ├── src/
│       └── package.json
├── packages/
│   ├── types/              ← Shared TypeScript types (API contracts)
│   ├── config/             ← Shared ESLint, Prettier, tsconfig
│   └── ui/                 ← Shared design system (optional)
├── .claude/                ← This fullstack config (root-level)
├── turbo.json
├── pnpm-workspace.yaml
└── package.json
```

### Option B: Next.js Fullstack (Single repo, API Routes or Server Actions)
```
project-root/
├── .claude/                ← This fullstack config
├── src/
│   ├── app/                ← Next.js App Router (pages + API routes)
│   │   ├── api/            ← Backend: API route handlers
│   │   │   └── v1/
│   │   ├── (auth)/         ← Frontend: auth pages
│   │   ├── (dashboard)/    ← Frontend: app pages
│   │   └── layout.tsx
│   ├── components/         ← Frontend: reusable UI
│   ├── features/           ← Frontend: feature modules
│   ├── server/             ← Backend: services, repositories, DB client
│   │   ├── services/
│   │   ├── repositories/
│   │   └── db.ts
│   ├── api/                ← Frontend: API call helpers (calls app/api/)
│   ├── lib/                ← Shared: utilities, Zod schemas, constants
│   └── types/              ← Shared: TypeScript types
├── prisma/                 ← Database schema + migrations
└── package.json
```

### Option C: Separate Repos (Independent deployments)
```
my-project-backend/         ← Deployed to Railway / Fly.io / AWS
├── .claude/                ← backend-specific Claude config
└── src/

my-project-frontend/        ← Deployed to Vercel / Cloudflare Pages
├── .claude/                ← frontend-specific Claude config
└── src/
```

---

## Backend Layer Responsibilities

```
Request → [ Routes ] → [ Middleware ] → [ Controllers ] → [ Services ] → [ Repositories ] → DB
```

| Layer | Owns | Must NOT |
|-------|------|---------|
| **Routes** | URL mapping, method binding | Contain logic |
| **Middleware** | Auth, validation, logging, rate-limit | Know about business rules |
| **Controllers** | Parse request, call service, format response | Contain business logic |
| **Services** | Business rules, orchestration | Know about HTTP or DB details |
| **Repositories** | Data access, queries | Contain business logic |

---

## Frontend Layer Responsibilities

```
Page → [ Feature Module ] → [ API Layer ] → Backend
              ↓
      [ Shared Components ]
```

| Layer | Owns | Must NOT |
|-------|------|---------|
| **Pages / Routes** | Layout, data fetching (SSR/SSG) | Business logic |
| **Feature Modules** | Feature components, hooks, state | Know other features' internals |
| **Shared Components** | Reusable UI primitives | Make API calls directly |
| **API Layer** | All HTTP calls, response mapping | Import UI components |

---

## Shared Code (Monorepo)

When using a monorepo, put in `packages/types/`:
- API request/response type definitions
- Zod schemas shared between frontend validation and backend validation
- Enum values used on both sides

```ts
// packages/types/src/user.ts
export interface User {
  id: string;
  email: string;
  role: 'USER' | 'ADMIN';
}

export interface CreateUserRequest {
  email: string;
  password: string;
  name: string;
}
```

---

## File Naming Conventions

**Backend (JS/TS)**: `kebab-case.ts` — `user-service.ts`, `auth-middleware.ts`  
**Frontend Components**: `PascalCase.tsx` — `UserList.tsx`, `LoginForm.tsx`  
**Frontend Hooks**: `camelCase.ts` prefixed `use` — `useAuth.ts`, `useTasks.ts`  
**Shared utilities**: `camelCase.ts` — `utils.ts`, `constants.ts`  
**Tests**: `[name].test.ts(x)` (unit/integration), `[name].spec.ts` (E2E)

---

## Environment Files

```
.env                ← Root dev (gitignored)
.env.example        ← Template committed to git
apps/backend/.env   ← Backend-specific (gitignored)
apps/frontend/.env.local ← Frontend-specific (gitignored)
```

Key variables to coordinate:
```bash
# Backend
PORT=3001
DATABASE_URL=...
JWT_SECRET=...
CORS_ORIGIN=http://localhost:3000

# Frontend
NEXT_PUBLIC_API_URL=http://localhost:3001
```
