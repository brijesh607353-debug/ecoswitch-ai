# Contributing to EcoSwitch AI

First off — **thank you** for taking the time to contribute! 🎉
Every bug report, feature idea, and line of code helps make EcoSwitch AI better.

This guide covers everything you need to know to get started.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
  - [Reporting Bugs](#reporting-bugs)
  - [Suggesting Features](#suggesting-features)
  - [Your First Code Contribution](#your-first-code-contribution)
- [Development Setup](#development-setup)
- [Branching Strategy](#branching-strategy)
- [Commit Message Convention](#commit-message-convention)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Project Structure](#project-structure)

---

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](./CODE_OF_CONDUCT.md).
By participating, you agree to uphold these standards. Please report unacceptable behaviour to the maintainers.

---

## How Can I Contribute?

### Reporting Bugs

Before submitting a bug, please check the [existing issues](https://github.com/YOUR_USERNAME/ecoswitch-ai/issues) to avoid duplicates.

When filing a bug report, include:

- **A clear, descriptive title**
- **Steps to reproduce** — the more detail, the better
- **Expected behaviour** vs **actual behaviour**
- **Environment** — OS, Node.js version, browser (if applicable)
- **Screenshots or logs** — attach them if relevant

### Suggesting Features

Feature requests are welcome! Open an issue with:

- **Problem statement** — what pain point does this solve?
- **Proposed solution** — your idea in plain language
- **Alternatives considered** — any other approaches you thought of

### Your First Code Contribution

Looking for a good first issue? Search for issues labelled:

- [`good first issue`](https://github.com/YOUR_USERNAME/ecoswitch-ai/labels/good%20first%20issue)
- [`help wanted`](https://github.com/YOUR_USERNAME/ecoswitch-ai/labels/help%20wanted)

---

## Development Setup

### Prerequisites

- **Node.js** ≥ 20
- **pnpm** ≥ 9 — `npm install -g pnpm`
- **PostgreSQL** ≥ 15

### Steps

```bash
# 1. Fork the repo on GitHub, then clone your fork
git clone https://github.com/YOUR_USERNAME/ecoswitch-ai.git
cd ecoswitch-ai

# 2. Add the upstream remote
git remote add upstream https://github.com/ORIGINAL_OWNER/ecoswitch-ai.git

# 3. Install dependencies
pnpm install

# 4. Set up environment variables
cp .env.example .env.local
# Edit .env.local with your local values

# 5. Push the database schema (development only)
pnpm --filter @workspace/db run push

# 6. Start the API server
pnpm --filter @workspace/api-server run dev
```

### Codegen

After editing `lib/api-spec/openapi.yaml`, regenerate the typed client:

```bash
pnpm --filter @workspace/api-spec run codegen
```

---

## Branching Strategy

| Branch | Purpose |
|--------|---------|
| `main` | Stable, production-ready code |
| `feat/*` | New features |
| `fix/*` | Bug fixes |
| `docs/*` | Documentation changes only |
| `chore/*` | Tooling, dependencies, refactoring |
| `test/*` | Test additions or fixes |

Always branch off `main` and target `main` in your PR.

---

## Commit Message Convention

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<optional scope>): <short summary>

[optional body]

[optional footer]
```

**Types:**

| Type | When to use |
|------|-------------|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation changes only |
| `style` | Formatting, whitespace (no logic change) |
| `refactor` | Code restructuring (no feature/fix) |
| `test` | Adding or fixing tests |
| `chore` | Build process, dependency updates |
| `perf` | Performance improvements |

**Examples:**

```
feat(api): add energy plan comparison endpoint
fix(db): resolve connection timeout on cold start
docs: update environment variable table in README
chore: bump drizzle-orm to v0.32
```

---

## Pull Request Process

1. **Sync with upstream** before opening a PR:
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Run the full typecheck** — PRs must pass:
   ```bash
   pnpm run typecheck
   ```

3. **Keep PRs focused** — one logical change per PR. Large PRs are hard to review.

4. **Fill in the PR template** — describe what changed and why.

5. **Link related issues** — use `Closes #123` in the PR description.

6. A maintainer will review within **5 business days**. Be responsive to feedback.

7. Once approved, a maintainer will **squash-merge** your PR.

---

## Coding Standards

- **TypeScript strict mode** is enabled — no `any`, no implicit `undefined`
- **Never use `console.log`** in server code — use `req.log` in route handlers or the `logger` singleton
- **Validate all inputs** — use Zod schemas (generated from the OpenAPI spec where possible)
- **Keep files small** — split large files into focused modules
- **Use `catalog:` pins** in `pnpm-workspace.yaml` for shared dependencies — check before adding a new dep

### Linting & Formatting

```bash
# Format with Prettier
pnpm prettier --write .

# TypeScript check
pnpm run typecheck
```

---

## Project Structure

See the [README Project Structure section](./README.md#project-structure) for a map of where things live.

Key rule: **`artifacts/*` packages must never import from each other** — shared logic belongs in `lib/*`.

---

Thank you again for contributing. You're awesome. 🌱
