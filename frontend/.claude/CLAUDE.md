# Frontend — Claude AI Agent Configuration

## Project Setup Checklist

> **Starting a new project? Complete this before asking Claude to write any code.**

- [ ] Fill out `rules/tech-stack.md` — framework, styling, state, data fetching, testing tools
- [ ] Update **This Project** section below with name and description
- [ ] Note rendering strategy (SSR/SSG for SEO vs SPA for admin)
- [ ] Note accessibility target (WCAG 2.1 AA is default)

Once `tech-stack.md` is filled, Claude will adapt all code examples, folder structure suggestions, and component patterns to your actual stack.

---

## This Project

**Name**: [Project Name]  
**Description**: [What this frontend does — 1-2 sentences]  
**Target users**: [e.g. Public visitors, Internal admin, End customers]  
**Special constraints**: [e.g. must support i18n, PWA, IE11, offline mode]

---

## Overview

Frontend application built with Next.js 14 (App Router), TypeScript, Tailwind CSS, shadcn/ui, Zustand, and TanStack Query. This Claude agent configuration governs all client-side development in this directory.

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

- **Server Components First** — Default to RSC, add `'use client'` only when needed
- **Mobile First** — Design for 320px viewport, enhance upward
- **Accessible by Default** — WCAG 2.1 AA minimum on every component
- **Performance Targets** — LCP < 2.5s, CLS < 0.1, INP < 200ms
- **TypeScript Strict** — Never use `any` without justification

---

## Mandatory Rules

All rules in `.claude/rules/` are **mandatory**:

### Frontend-Relevant
| Rule | Description |
|------|-------------|
| `tech-stack.md` | Next.js vs React+Vite decision, shadcn/ui, Tailwind, Zustand |
| `project-structure.md` | App Router layout, features/, components/ organization |
| `error-handling.md` | Error boundaries, error states in components |

### Shared (Code Quality)
| Rule | Description |
|------|-------------|
| `clean-code.md` | Variables, functions, SOLID, async/await |
| `code-style.md` | Formatting, naming conventions |
| `security.md` | **CRITICAL** — XSS prevention, input sanitization |
| `testing.md` | Coverage thresholds, Testing Library patterns |
| `git-workflow.md` | Branching strategy, conventional commits |

### Architecture & Design
| Rule | Description |
|------|-------------|
| `naming-conventions.md` | File naming, component naming, env vars |
| `system-design.md` | Caching strategies, state management decisions |

---

## Available Agents

### Primary
| Agent | When to Invoke |
|-------|---------------|
| 🖥️ **Frontend Developer** | Components, pages, routing, state, UI performance |
| 🎨 **UI/UX Designer** | Design system, wireframes, Tailwind tokens, accessibility |
| ✍️ **Copywriter/SEO** | Page copy, meta tags, OpenGraph, SEO optimization |

### Quality
| Agent | When to Invoke |
|-------|---------------|
| 👀 **Code Reviewer** | Five-axis PR review |
| 🧪 **Test Engineer** | Vitest + Testing Library + Playwright |
| 🔒 **Security Auditor** | XSS, CSP, dependency audit |
| ✅ **QA Engineer** | E2E test plans, Playwright scripts, accessibility testing |

### Product
| Agent | When to Invoke |
|-------|---------------|
| 📋 **Project Manager** | User stories, sprint planning |

---

## Available Skills

| Skill | Description |
|-------|-------------|
| `tdd` | Test-Driven Development with Testing Library |
| `code-review` | Five-axis review framework |
| `incremental-implementation` | Vertical slice, component-first development |
| `deploy` | Vercel deployment pipeline |
| `security-review` | XSS/CSP/dependency security audit |

---

## Reference Checklists

| Reference | Use For |
|-----------|---------|
| `GUIDE.md` | Hướng dẫn sử dụng commands, cách thêm tính năng và fix bug |
| `accessibility-checklist.md` | WCAG 2.1 AA — must check before every PR |
| `performance-checklist.md` | Core Web Vitals, bundle size, image optimization |
| `security-checklist.md` | XSS, CSP, secrets, dependency audit |
| `testing-patterns.md` | Testing Library patterns, Playwright E2E |

---

## Tech Stack Summary

```
Framework:   Next.js 14+ (App Router) or React + Vite (Admin SPA)
Language:    TypeScript 5 (strict mode)
Styling:     Tailwind CSS + shadcn/ui + Radix UI
State:       Zustand (global) + useState/useReducer (local)
Server State: TanStack Query (React Query)
Forms:       React Hook Form + Zod
Testing:     Vitest + Testing Library + Playwright (E2E)
Deploy:      Vercel
```

---

## Agent Behavior Guidelines

1. **Follow the workflow** — `/spec` → `/plan` → `/build` → `/review`
2. **Apply mandatory rules** — All rules in `.claude/rules/` are non-negotiable
3. **Test first** — Write failing tests before implementing
4. **Accessibility always** — Check `accessibility-checklist.md` before every PR
5. **Server Components by default** — Only use `'use client'` when strictly needed
6. **Fix root causes** — Don't patch symptoms
7. **Use the right agent** — Frontend for UI, UI/UX for design, Copywriter for content
