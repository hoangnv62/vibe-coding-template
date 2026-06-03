# Backend — Claude AI Agent Configuration

## Project Setup Checklist

> **Starting a new project? Complete this before asking Claude to write any code.**

- [ ] Fill out `rules/tech-stack.md` — framework, database, ORM, auth, testing tools
- [ ] Update **This Project** section below with name and description
- [ ] Note any non-standard constraints (team size, compliance, scale)

Once `tech-stack.md` is filled, Claude will adapt all code examples, folder structure suggestions, and tool recommendations to your actual stack.

---

## This Project

**Name**: [Project Name]  
**Description**: [What this backend does — 1-2 sentences]  
**Special constraints**: [e.g. must support multi-tenant, GDPR, < 100ms p99 latency]

---

## Overview

Backend service built with Node.js, Express, PostgreSQL, Redis, and BullMQ. This Claude agent configuration governs all server-side development in this directory.

---

## Development Workflow

```
/spec  →  /plan  →  /build  →  /test  →  /review  →  /deploy
Define    Plan     Build     Verify    Review      Ship
```

| Phase | Command | Purpose |
|-------|---------|---------|
| **Define** | `/spec` | Create PRD with objectives, scope, boundaries |
| **Plan** | `/plan` | Decompose into vertical slices with acceptance criteria |
| **Build** | `/build` | Implement incrementally using TDD (RED-GREEN-REFACTOR) |
| **Verify** | `/test` | Write and verify tests; use Prove-It for bug fixes |
| **Review** | `/review` | Five-axis code review before merge |
| **Ship** | `/deploy` | Build, test, deploy with staged rollout |

### Supporting Commands

| Command | Purpose |
|---------|---------|
| `/debug` | Systematic error diagnosis and root cause analysis |
| `/simplify` | Reduce complexity without changing behavior |
| `/fix-issue` | Analyze and fix reported issues |

---

## Core Principles

- **Test-Driven Development** — Write failing tests first, then implement
- **Layered Architecture** — Route → Controller → Service → Repository → Database
- **Security First** — Validate inputs, parameterized queries, auth on all protected routes
- **Observability** — Structured logging (Pino), Prometheus metrics, distributed tracing

---

## Mandatory Rules

All rules in `.claude/rules/` are **mandatory**:

### Backend-Specific
| Rule | Description |
|------|-------------|
| `api-conventions.md` | REST standards, response envelopes, HTTP status codes |
| `database.md` | Prisma patterns, transactions, N+1 prevention |
| `monitoring.md` | Prometheus, Grafana, logging, alerting |
| `error-handling.md` | AppError class, global handler, async wrapper |

### Shared (Code Quality)
| Rule | Description |
|------|-------------|
| `clean-code.md` | Variables, functions, SOLID, async/await |
| `code-style.md` | Formatting, naming conventions |
| `security.md` | **CRITICAL** — Never violate security rules |
| `testing.md` | Coverage thresholds, test patterns |
| `git-workflow.md` | Branching strategy, conventional commits |

### Architecture & Design
| Rule | Description |
|------|-------------|
| `tech-stack.md` | Approved technologies (Express, PG, Redis, Prisma, BullMQ) |
| `system-design.md` | CAP theorem, caching, scaling, queues |
| `project-structure.md` | Layered architecture, folder organization |
| `naming-conventions.md` | Cache keys, DB, queues, env vars |

---

## Available Agents

### Primary
| Agent | When to Invoke |
|-------|---------------|
| 🔧 **Backend Developer** | APIs, services, DB queries, background jobs |
| 🏗️ **Systems Architect** | Architecture decisions, ADRs, system design |

### Quality
| Agent | When to Invoke |
|-------|---------------|
| 👀 **Code Reviewer** | Five-axis PR review |
| 🧪 **Test Engineer** | Test strategy, TDD, coverage |
| 🔒 **Security Auditor** | Vulnerability assessment, threat modeling |
| ✅ **QA Engineer** | Test plans, E2E tests, bug reports |

### Product
| Agent | When to Invoke |
|-------|---------------|
| 📋 **Project Manager** | User stories, sprint planning |

---

## Available Skills

| Skill | Description |
|-------|-------------|
| `tdd` | Test-Driven Development patterns |
| `code-review` | Five-axis review framework |
| `incremental-implementation` | Vertical slice development |
| `deploy` | Full deployment pipeline |
| `security-review` | Security audit checklist |

---

## Reference Checklists

| Reference | Use For |
|-----------|---------|
| `GUIDE.md` | Hướng dẫn sử dụng commands, cách thêm tính năng và fix bug |
| `security-checklist.md` | Pre-deploy security verification |
| `testing-patterns.md` | Test structure and anti-patterns |
| `performance-checklist.md` | Query optimization, caching |

---

## Tech Stack Summary

```
Runtime:    Node.js 20 LTS + TypeScript 5 (strict)
Framework:  Express.js
ORM:        Prisma + PostgreSQL 16
Cache:      Redis (ioredis)
Queue:      BullMQ (simple) / RabbitMQ (microservices)
Auth:       JWT (15m access + 7d refresh) + bcrypt (12 rounds)
Logging:    Pino (structured JSON)
Testing:    Vitest + Supertest
Deploy:     Docker + Railway/Fly.io
```

---

## Agent Behavior Guidelines

1. **Follow the workflow** — `/spec` → `/plan` → `/build` → `/review`
2. **Apply mandatory rules** — All rules in `.claude/rules/` are non-negotiable
3. **Test first** — Write failing tests before implementing
4. **Never expose internals** — Use AppError for operational errors only
5. **Validate at the boundary** — All inputs validated with Zod in controllers
6. **Fix root causes** — Don't patch symptoms
