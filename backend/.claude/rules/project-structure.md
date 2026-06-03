# Project Structure

> Folder names vary by framework — see `tech-stack.md` for your actual choices.  
> The **architectural layers** below are the non-negotiable part.

---

## Layered Architecture (The Pattern)

```
Request
  ↓
[ Transport Layer ]     — HTTP routes, WebSocket handlers, CLI commands
  ↓
[ Application Layer ]   — Use cases, input validation, orchestration
  ↓
[ Domain Layer ]        — Business rules, entities, domain events
  ↓
[ Infrastructure Layer ]— Database, cache, queue, external APIs, email
```

### Layer Responsibilities

| Layer | Owns | Must NOT |
|-------|------|---------|
| **Transport** | Routing, auth middleware, request parsing | Contain business logic |
| **Application** | Orchestrate services, validate input | Know about DB or HTTP details |
| **Domain** | Business rules, entities, invariants | Import from infrastructure |
| **Infrastructure** | All I/O (DB, cache, queue, storage) | Contain business logic |

### Dependency Rule

```
Transport → Application → Domain
    ↑              ↑          ↑
Infrastructure can be used by any layer above it,
but Infrastructure must NEVER import from Domain or Application.
```

---

## Folder Mapping by Framework

> Choose your framework in `tech-stack.md`, then use the corresponding structure.

### Express / Fastify / Hono
```
src/
├── routes/           ← Transport: route definitions
├── controllers/      ← Transport: request/response handling
├── middleware/       ← Transport: auth, validation, logging
├── services/         ← Application: business orchestration
├── repositories/     ← Domain/Infra: data access abstraction
├── models/           ← Domain: entities, types
├── lib/              ← Infrastructure: DB client, Redis, queue
├── config/           ← Configuration, env validation
└── index.ts          ← Entry point
```

### NestJS
```
src/
├── modules/          ← Feature modules (auth, users, orders...)
│   └── users/
│       ├── users.controller.ts
│       ├── users.service.ts
│       ├── users.repository.ts
│       └── dto/
├── common/           ← Guards, interceptors, decorators, pipes
├── config/           ← ConfigModule setup
└── main.ts           ← Entry point
```

### FastAPI (Python)
```
app/
├── api/              ← Routes and dependencies
│   └── v1/
├── core/             ← Config, security, logging
├── models/           ← SQLAlchemy models
├── schemas/          ← Pydantic schemas (request/response)
├── services/         ← Business logic
├── repositories/     ← Data access
└── main.py           ← Entry point
```

### Serverless (Lambda / Vercel Functions)
```
functions/            ← One file per endpoint
├── users/
│   ├── get.ts
│   ├── create.ts
│   └── [id]/
│       └── update.ts
├── lib/              ← Shared utilities (DB client, etc.)
└── middleware/       ← Shared middleware
```

---

## Universal Conventions

Regardless of framework:

```
tests/
├── unit/             ← Isolated logic, no I/O
├── integration/      ← Real DB/HTTP, no mocks
└── e2e/              ← Full flow from HTTP to DB

docs/
├── api/              ← API reference (OpenAPI YAML or generated)
└── architecture/     ← Architecture decision records (ADRs)
```

### File Naming (adapt to language conventions)
- **JS/TS**: `kebab-case.ts` — `user-service.ts`, `auth-middleware.ts`
- **Python**: `snake_case.py` — `user_service.py`, `auth_middleware.py`
- **Go**: `snake_case.go` — `user_service.go`
- Test files: mirror the source file name + `.test.ts` / `_test.py` / `_test.go`

### Environment Files
```
.env              ← Local dev (gitignored)
.env.example      ← Template committed to git
.env.test         ← Test env (gitignored)
.env.production   ← Set in CI/CD, never committed
```

---

## What Goes Where — Quick Reference

| If it... | Put it in... |
|----------|-------------|
| Handles HTTP request/response | Transport layer (routes/controllers) |
| Validates request shape | Transport layer (middleware/validators) |
| Orchestrates multiple services | Application layer (services) |
| Contains a business rule | Domain layer (services/entities) |
| Touches the database | Infrastructure (repository/ORM) |
| Connects to Redis, S3, email | Infrastructure (lib/) |
| Runs on a schedule | Jobs/cron (can be a separate module) |
| Processes async messages | Queue workers (infrastructure) |
| Used everywhere | Shared/common utilities |
