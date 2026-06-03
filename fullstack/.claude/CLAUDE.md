# Fullstack — Claude AI Agent Configuration

## Project Setup Checklist

> **Starting a new project? Complete this before asking Claude to write any code.**

- [ ] Fill out `rules/tech-stack.md` — backend stack, frontend stack, monorepo tool, auth flow
- [ ] Update **This Project** section below
- [ ] Decide on monorepo vs separate repos (see `rules/project-structure.md`)
- [ ] Note any non-standard constraints (compliance, scale, i18n, offline)

Once `tech-stack.md` is filled, Claude will adapt all code examples and architecture suggestions to your actual stack — both backend and frontend.

---

## This Project

**Name**: [Project Name]  
**Description**: [What this app does — 1-2 sentences]  
**Repo structure**: [ ] Monorepo (Turborepo/pnpm) / [ ] Next.js fullstack / [ ] Separate repos  
**Special constraints**: [e.g. GDPR, multi-tenant, must support mobile]

---

## Development Workflow

```
/spec  →  /plan  →  /build  →  /test  →  /review  →  /deploy
Define    Plan     Build     Verify    Review      Ship
```

| Phase | Command | Purpose |
|-------|---------|---------|
| **Define** | `/spec` | Create PRD — user stories, API contracts, UI flows |
| **Plan** | `/plan` | Decompose into vertical slices (backend + frontend together) |
| **Build** | `/build` | Implement incrementally: API endpoint → hook → UI component |
| **Verify** | `/test` | Unit + integration (backend) + component + E2E (frontend) |
| **Review** | `/review` | Five-axis review: correctness, security, performance, UX, arch |
| **Ship** | `/deploy` | Build, test, deploy backend + frontend |

### Supporting Commands
| Command | Purpose |
|---------|---------|
| `/debug` | Systematic error diagnosis (backend error or frontend UI bug) |
| `/simplify` | Reduce complexity without changing behavior |
| `/fix-issue` | Analyze and fix reported issues |

---

## Fullstack Build Order

When implementing a feature, always follow this order:

```
1. Database schema / migration
2. Repository (data access)
3. Service (business logic)
4. API endpoint (route + controller)
         ↓ agreed contract ↓
5. TypeScript types (shared)
6. API client function (frontend)
7. TanStack Query hook
8. UI Component (with loading + error states)
9. E2E test (full flow)
```

> Never build the UI before the API contract is defined, even if the backend isn't implemented yet.  
> Use mock data on the frontend while the backend is being built.

---

## Mandatory Rules

All rules in `.claude/rules/` are **mandatory**:

### Fullstack-Specific
| Rule | Description |
|------|-------------|
| `tech-stack.md` | **Fill this out first** — all tool choices, backend + frontend |
| `project-structure.md` | Monorepo patterns, layered arch, component architecture |
| `error-handling.md` | AppError (backend) + Error Boundaries (frontend) + agreed error contract |
| `security.md` | Backend (Helmet, JWT, bcrypt) + Frontend (XSS, token storage, CSP) |
| `testing.md` | Backend (Supertest) + Frontend (Testing Library) + E2E (Playwright) |

### Backend Rules
| Rule | Description |
|------|-------------|
| `api-conventions.md` | REST standards, response envelopes, HTTP status codes |
| `database.md` | ORM patterns, transactions, N+1 prevention |
| `monitoring.md` | Logging, Prometheus metrics, distributed tracing |

### Shared Rules
| Rule | Description |
|------|-------------|
| `clean-code.md` | Variables, functions, SOLID |
| `code-style.md` | Formatting, naming |
| `security.md` | **CRITICAL** — both layers |
| `naming-conventions.md` | Cache keys, DB, env vars, file names |
| `git-workflow.md` | Branching, conventional commits |
| `system-design.md` | CAP theorem, caching, scaling, queues |

---

## Available Agents

### Development
| Agent | When to Invoke |
|-------|---------------|
| 🔧 **Backend Developer** | API endpoints, services, DB queries, background jobs, auth |
| 🖥️ **Frontend Developer** | Components, pages, routing, state, forms, UI performance |
| 🏗️ **Systems Architect** | Architecture decisions, ADRs, fullstack design, monorepo setup |

### Quality
| Agent | When to Invoke |
|-------|---------------|
| 👀 **Code Reviewer** | Five-axis PR review (correctness, security, arch, readability, performance) |
| 🧪 **Test Engineer** | TDD, coverage strategy, E2E test design |
| 🔒 **Security Auditor** | Threat modeling, vulnerability assessment (both layers) |
| ✅ **QA Engineer** | Test plans, E2E scripts, accessibility testing |

### Product & Design
| Agent | When to Invoke |
|-------|---------------|
| 📋 **Project Manager** | User stories, sprint planning, task breakdown |
| 🎨 **UI/UX Designer** | Design system, wireframes, accessibility, component design |
| ✍️ **Copywriter/SEO** | Page copy, meta tags, OpenGraph, SEO optimization |

---

## Available Skills

| Skill | Description |
|-------|-------------|
| `tdd` | Test-Driven Development (RED → GREEN → REFACTOR) |
| `code-review` | Five-axis review framework |
| `incremental-implementation` | Vertical slice development (API + UI together) |
| `deploy` | Full deployment pipeline (backend + frontend) |
| `security-review` | Security audit across both layers |

---

## Reference Checklists

| Reference | Use For |
|-----------|---------|
| `GUIDE.md` | Hướng dẫn sử dụng commands, cách thêm tính năng và fix bug |
| `security-checklist.md` | Pre-deploy security verification (backend + frontend) |
| `testing-patterns.md` | Test structure and anti-patterns |
| `performance-checklist.md` | API optimization + Core Web Vitals |
| `accessibility-checklist.md` | WCAG 2.1 AA — check before every frontend PR |

---

## Agent Behavior Guidelines

1. **Check `tech-stack.md` first** — never assume a framework or library
2. **Follow the workflow** — `/spec` → `/plan` → `/build` → `/review`
3. **Vertical slices** — implement API + frontend together, not in separate phases
4. **Test first** — write failing tests before implementing
5. **Agree on the contract** — define API request/response types before building either side
6. **Fix root causes** — don't patch symptoms
7. **Security on both layers** — validate on frontend (UX) AND backend (security)
8. **Use the right agent** — Backend for API, Frontend for UI, Architect for decisions
