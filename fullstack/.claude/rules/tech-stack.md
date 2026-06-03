# Tech Stack — [Project Name]

> **Fill this out at project start.**  
> Claude uses this file as the source of truth for ALL tool recommendations — both backend and frontend.  
> Leave a `?` next to anything not yet decided.

---

## Backend Stack

| Layer | Technology | Notes |
|-------|-----------|-------|
| **Runtime** | | e.g. Node.js 20, Deno, Bun, Python, Go |
| **Language** | | e.g. TypeScript (strict), Python, Go |
| **Framework** | | e.g. Express, Fastify, NestJS, Hono, FastAPI, Django |
| **Database** | | e.g. PostgreSQL, MySQL, MongoDB, SQLite |
| **ORM / Query Builder** | | e.g. Prisma, Drizzle, TypeORM, Mongoose, SQLAlchemy |
| **Cache** | | e.g. Redis, Memcached, None |
| **Queue** | | e.g. BullMQ, RabbitMQ, Kafka, None |
| **Auth** | | e.g. JWT + bcrypt, Sessions, OAuth, Clerk, Auth0 |
| **File Storage** | | e.g. AWS S3, Cloudflare R2, Local (dev only) |
| **Email** | | e.g. Resend, Nodemailer, SendGrid, None |
| **API Style** | | e.g. REST, GraphQL, tRPC, gRPC |
| **API Docs** | | e.g. Swagger/OpenAPI, None |
| **Logging** | | e.g. Pino, Winston, structlog |
| **Monitoring** | | e.g. Prometheus + Grafana, Datadog, Sentry, None |
| **Testing** | | e.g. Vitest + Supertest, Jest, Pytest |
| **Deploy** | | e.g. Railway, Fly.io, AWS, GCP, Docker |

---

## Frontend Stack

| Layer | Technology | Notes |
|-------|-----------|-------|
| **Framework** | | e.g. Next.js 14 App Router, React + Vite, Remix, Nuxt, SvelteKit |
| **Language** | | e.g. TypeScript (strict), JavaScript |
| **Styling** | | e.g. Tailwind CSS, CSS Modules, Styled-components |
| **Component Library** | | e.g. shadcn/ui, MUI, Chakra UI, None |
| **Global State** | | e.g. Zustand, Redux Toolkit, Jotai, Context API, None |
| **Server State / Data Fetching** | | e.g. TanStack Query, SWR, RTK Query, fetch (native) |
| **Forms** | | e.g. React Hook Form, Formik, None |
| **Validation** | | e.g. Zod, Yup, Valibot |
| **Animation** | | e.g. Framer Motion, GSAP, CSS, None |
| **Auth (client-side)** | | e.g. NextAuth, Lucia, Clerk, Custom JWT |
| **API Client** | | e.g. Axios, fetch, ky, ofetch |
| **Testing** | | e.g. Vitest + Testing Library, Playwright (E2E) |
| **Deploy** | | e.g. Vercel, Cloudflare Pages, Netlify |

---

## Integration

| | Detail |
|--|--------|
| **Monorepo tool** | e.g. Turborepo, pnpm workspaces, Nx, None (separate repos) |
| **API base URL (dev)** | e.g. `http://localhost:3001` |
| **API base URL (prod)** | e.g. `https://api.example.com` |
| **Auth token storage** | e.g. httpOnly cookie (recommended), memory, localStorage |
| **Shared types** | e.g. packages/types (monorepo), OpenAPI codegen, copy |
| **CI/CD** | e.g. GitHub Actions, GitLab CI |

---

## Architecture Pattern

Mark the pattern you're using:

**Backend:**
- [ ] Layered — Controller → Service → Repository → Database
- [ ] Clean / Hexagonal — Domain at center, ports & adapters
- [ ] Modular Monolith — Feature modules with shared kernel
- [ ] Serverless — Functions per endpoint

**Frontend:**
- [ ] Full SSR/SSG (Next.js App Router)
- [ ] SPA (React + Vite, admin/internal)
- [ ] Mixed (SSG for public + SPA for dashboard)

---

## Project Context

| | Detail |
|--|--------|
| **Team size** | |
| **Timeline** | |
| **Scale expectation** | e.g. < 1k users, 10k RPS, millions of records |
| **Target users** | e.g. Public visitors + admin panel |
| **SEO required?** | Yes / No |
| **Special requirements** | e.g. multi-tenant, GDPR, i18n, PWA, offline |

---

## Why These Choices

<!-- Brief notes on any non-obvious decisions -->

---

## Decision Log

| Date | Decision | Reason |
|------|---------|--------|
| | | |

---

> **Note for Claude:** Always check this file before generating code.  
> Use backend stack for server-side code, frontend stack for client-side code.  
> If a row is blank or has `?`, ask the user before assuming a choice.
