# Tech Stack — [Project Name]

> **Fill this out at project start.**  
> Claude uses this file as the source of truth for all tool recommendations and code examples.  
> Leave a `?` next to anything not yet decided.

---

## Stack Decisions

| Layer | Technology | Notes |
|-------|-----------|-------|
| **Runtime** | | e.g. Node.js 20, Deno, Bun |
| **Language** | | e.g. TypeScript (strict), JavaScript, Python, Go |
| **Framework** | | e.g. Express, Fastify, NestJS, Hono, FastAPI, Django |
| **Database** | | e.g. PostgreSQL, MySQL, MongoDB, SQLite |
| **ORM / Query Builder** | | e.g. Prisma, Drizzle, TypeORM, Mongoose, SQLAlchemy, raw SQL |
| **Cache** | | e.g. Redis, Memcached, None |
| **Queue** | | e.g. BullMQ, RabbitMQ, Kafka, None |
| **Auth** | | e.g. JWT + bcrypt, Sessions, OAuth, Clerk, Auth0 |
| **File Storage** | | e.g. AWS S3, Cloudflare R2, Local (dev only) |
| **Email** | | e.g. Resend, Nodemailer, SendGrid, None |
| **API Style** | | e.g. REST, GraphQL, tRPC, gRPC |
| **API Docs** | | e.g. Swagger/OpenAPI, None |
| **Logging** | | e.g. Pino, Winston, console (dev only) |
| **Monitoring** | | e.g. Prometheus + Grafana, Datadog, Sentry, None |
| **Testing** | | e.g. Vitest + Supertest, Jest, Pytest |
| **Containerization** | | e.g. Docker + Compose, None |
| **Deploy** | | e.g. Railway, Fly.io, AWS, GCP, Vercel |
| **CI/CD** | | e.g. GitHub Actions, GitLab CI |

---

## Architecture Pattern

Mark the pattern you're using:

- [ ] **Layered** — Controller → Service → Repository → Database
- [ ] **Clean / Hexagonal** — Domain at center, ports & adapters at edges
- [ ] **Modular Monolith** — Feature modules with shared kernel
- [ ] **Microservices** — Independent services per domain
- [ ] **Serverless** — Functions per endpoint (Lambda, Vercel Functions)

---

## Project Constraints

| Constraint | Detail |
|-----------|--------|
| **Team size** | |
| **Timeline** | |
| **Scale expectation** | e.g. < 1k users, 10k RPS, millions of records |
| **Special requirements** | e.g. multi-tenant, GDPR, offline support |

---

## Why These Choices

<!-- Add a note for any non-obvious decision -->
<!-- Example: Using SQLite instead of PostgreSQL because this is a local-first desktop app -->

---

## Decision Log

| Date | Decision | Reason |
|------|---------|--------|
| | | |

---

> **Note for Claude:** When generating code examples, use the technologies listed above.  
> If a row is blank or has `?`, ask the user before assuming a choice.
