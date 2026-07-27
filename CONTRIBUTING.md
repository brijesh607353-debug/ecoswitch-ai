# Contributing to EcoSwitch AI

First off — **thank you** for taking the time to contribute! 🎉
EcoSwitch AI is a smart energy waste reduction system built for the vivo Ignite Innovation Challenge 2026. Every bug report, feature idea, and line of code helps make it better.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Branching Strategy](#branching-strategy)
- [Commit Message Convention](#commit-message-convention)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)

---

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](./CODE_OF_CONDUCT.md). By participating, you agree to uphold these standards.

---

## How Can I Contribute?

### Reporting Bugs

Check [existing issues](https://github.com/brijesh607353-debug/ecoswitch-ai/issues) first.
Use the **Bug Report** issue template and include steps to reproduce, expected vs. actual behaviour, and your environment details.

> **Do not report security vulnerabilities in public issues** — see [SECURITY.md](./SECURITY.md).

### Suggesting Features

Use the **Feature Request** issue template. Be clear about the problem it solves and who benefits.

### Good First Issues

Look for issues labelled:
- [`good first issue`](https://github.com/brijesh607353-debug/ecoswitch-ai/labels/good%20first%20issue)
- [`help wanted`](https://github.com/brijesh607353-debug/ecoswitch-ai/labels/help%20wanted)

---

## Development Setup

### Prerequisites

- **Node.js** ≥ 20 ([download](https://nodejs.org/))
- **pnpm** ≥ 9 — `npm install -g pnpm`
- **PostgreSQL** ≥ 15 ([download](https://www.postgresql.org/download/))

### Steps

```bash
# 1. Fork the repo on GitHub, then clone your fork
git clone https://github.com/YOUR_USERNAME/ecoswitch-ai.git
cd ecoswitch-ai

# 2. Add the upstream remote
git remote add upstream https://github.com/brijesh607353-debug/ecoswitch-ai.git

# 3. Install all workspace dependencies
pnpm install

# 4. Set up environment variables
cp .env.example .env.local
# Fill in DATABASE_URL and SESSION_SECRET at minimum

# 5. Push DB schema to your local database
pnpm --filter @workspace/db run push

# 6. Start the API server
pnpm --filter @workspace/api-server run dev

# 7. Verify it's working
curl http://localhost:5000/api/healthz
# → {"status":"ok"}
```

### After Editing the OpenAPI Spec

```bash
# Regenerate React Query hooks and Zod schemas
pnpm --filter @workspace/api-spec run codegen
```

Do not edit generated files in `lib/api-client-react/src/generated/` or `lib/api-zod/src/generated/` — they are overwritten on every codegen run.

---

## Branching Strategy

| Branch | Purpose |
|--------|---------|
| `main` | Stable, production-ready |
| `feat/*` | New features |
| `fix/*` | Bug fixes |
| `docs/*` | Documentation only |
| `chore/*` | Tooling, deps, refactoring |
| `test/*` | Tests |

Always branch from `main` and target `main` in your PR.

---

## Commit Message Convention

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <short summary>
```

| Type | When to use |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, whitespace |
| `refactor` | Code restructuring (no logic change) |
| `test` | Adding or fixing tests |
| `chore` | Build, deps, tooling |
| `perf` | Performance improvement |

**Examples:**
```
feat(api): add POST /api/readings endpoint for ESP32 sensor data
fix(db): resolve connection pool exhaustion under load
docs(iot): add circuit wiring diagram to IOT.md
chore: bump drizzle-orm to v0.33
```

---

## Pull Request Process

1. **Sync with upstream** before opening a PR:
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **TypeScript must pass:**
   ```bash
   pnpm run typecheck
   ```

3. **Run codegen** if you edited the OpenAPI spec:
   ```bash
   pnpm --filter @workspace/api-spec run codegen
   ```

4. **Fill in the PR template** — describe what changed and why.

5. **Link related issues** — use `Closes #123` in the description.

6. Keep PRs focused — one logical change per PR.

7. A maintainer will review within **5 business days**.

---

## Coding Standards

- **TypeScript strict mode** — no `any`, no implicit `undefined`
- **Never `console.log` in server code** — use `req.log` in route handlers, `logger` singleton elsewhere (see `artifacts/api-server/src/lib/logger.ts`)
- **Validate all inputs** — use Zod schemas (generated from OpenAPI where possible, or hand-written for internal logic)
- **Contract-first** — define new API endpoints in `openapi.yaml` before implementing the handler
- **Shared logic in `lib/*`** — `artifacts/*` must never import from each other
- **`catalog:` pins** — check `pnpm-workspace.yaml` before adding a new dependency; use `catalog:` if the package is already pinned

### Formatting

```bash
# Format all files
pnpm prettier --write .

# Check formatting (CI uses this)
pnpm prettier --check .
```

---

Thank you for helping build EcoSwitch AI! 🌱
