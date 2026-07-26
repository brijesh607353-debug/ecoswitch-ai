<div align="center">

# ⚡ EcoSwitch AI

**Intelligent energy switching powered by AI — helping users find greener, cheaper energy plans effortlessly.**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-blue?logo=typescript)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-24-brightgreen?logo=node.js)](https://nodejs.org/)
[![pnpm](https://img.shields.io/badge/pnpm-workspace-orange?logo=pnpm)](https://pnpm.io/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./CONTRIBUTING.md)
[![Code of Conduct](https://img.shields.io/badge/Contributor%20Covenant-2.1-4baaaa.svg)](./CODE_OF_CONDUCT.md)

<br />

<!-- Replace the line below with a real screenshot once the UI is ready -->
<!-- ![EcoSwitch AI Screenshot](./screenshots/hero.png) -->

[🚀 Live Demo](#) · [📖 Documentation](./docs/) · [🐛 Report Bug](https://github.com/YOUR_USERNAME/ecoswitch-ai/issues/new?template=bug_report.md) · [✨ Request Feature](https://github.com/YOUR_USERNAME/ecoswitch-ai/issues/new?template=feature_request.md)

</div>

---

## 📋 Table of Contents

- [About the Project](#about-the-project)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Variables](#environment-variables)
  - [Running Locally](#running-locally)
- [Project Structure](#project-structure)
- [API Reference](#api-reference)
- [Contributing](#contributing)
- [Security](#security)
- [Changelog](#changelog)
- [License](#license)

---

## 🌍 About the Project

**EcoSwitch AI** helps households and small businesses identify the most cost-effective and eco-friendly energy plans available to them. By combining real-time tariff data with AI-driven recommendations, EcoSwitch AI removes the friction from switching energy suppliers — saving money and reducing carbon footprint at the same time.

> **Built with a contract-first API design:** every endpoint is defined in an OpenAPI spec, auto-generating type-safe React Query hooks and Zod validation schemas throughout the stack.

---

## ✨ Features

- 🔋 **AI-powered plan comparison** — personalised energy plan recommendations
- 🌱 **Carbon footprint tracking** — estimate and reduce household emissions
- 💸 **Savings calculator** — project annual savings before switching
- 🔒 **Secure by default** — environment-based secrets, no credentials in code
- 📡 **Contract-first API** — OpenAPI spec drives codegen for the entire client
- 🧩 **Modular monorepo** — cleanly separated frontend, backend, and shared libs
- 📊 **Real-time health monitoring** — `/api/healthz` endpoint out of the box

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|-----------|
| **Runtime** | Node.js 24 |
| **Language** | TypeScript 5.9 (strict) |
| **API Server** | Express 5 |
| **Database** | PostgreSQL + Drizzle ORM |
| **Validation** | Zod v4 + drizzle-zod |
| **API Contract** | OpenAPI 3.1 + Orval codegen |
| **Frontend** | React + Vite |
| **Package Manager** | pnpm workspaces |
| **Logging** | Pino |

---

## 🚀 Getting Started

### Prerequisites

Make sure you have the following installed:

- **Node.js** ≥ 20 ([download](https://nodejs.org/))
- **pnpm** ≥ 9 — `npm install -g pnpm`
- **PostgreSQL** ≥ 15 ([download](https://www.postgresql.org/download/))

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/ecoswitch-ai.git
cd ecoswitch-ai

# 2. Install all workspace dependencies
pnpm install
```

### Environment Variables

Copy the example file and fill in your values:

```bash
cp .env.example .env.local
```

See [`.env.example`](./.env.example) for the full list of required variables and their descriptions.

### Running Locally

```bash
# Start the API server (port 5000 by default)
pnpm --filter @workspace/api-server run dev

# Regenerate API hooks & Zod schemas after spec changes
pnpm --filter @workspace/api-spec run codegen

# Push DB schema changes (development only)
pnpm --filter @workspace/db run push

# Full TypeScript check across all packages
pnpm run typecheck

# Build all packages
pnpm run build
```

---

## 📁 Project Structure

```
ecoswitch-ai/
├── artifacts/
│   ├── api-server/          # Express 5 API server
│   │   └── src/
│   │       ├── routes/      # Route handlers (auto-typed from OpenAPI)
│   │       ├── middlewares/ # Auth, logging, validation
│   │       └── lib/         # Shared server utilities
│   └── mockup-sandbox/      # React + Vite UI component sandbox
├── lib/
│   ├── api-spec/            # OpenAPI 3.1 spec (source of truth)
│   ├── api-client-react/    # Generated React Query hooks
│   ├── api-zod/             # Generated Zod validation schemas
│   └── db/                  # Drizzle ORM schema & migrations
├── scripts/                 # Utility/automation scripts
├── docs/                    # Extended documentation
├── screenshots/             # App screenshots for README
├── .env.example             # Required environment variables (no secrets)
├── pnpm-workspace.yaml      # Workspace package config & catalog
└── tsconfig.base.json       # Shared strict TypeScript config
```

---

## 📡 API Reference

The full API is described in [`lib/api-spec/openapi.yaml`](./lib/api-spec/openapi.yaml).

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/healthz` | Health check — returns server status |

> Additional endpoints are documented in the OpenAPI spec and auto-generate typed client hooks via `pnpm --filter @workspace/api-spec run codegen`.

---

## 🤝 Contributing

Contributions are what make open source amazing. Please read our [Contributing Guide](./CONTRIBUTING.md) before opening a pull request.

1. Fork the repository
2. Create your feature branch: `git checkout -b feat/amazing-feature`
3. Commit your changes: `git commit -m 'feat: add amazing feature'`
4. Push to the branch: `git push origin feat/amazing-feature`
5. Open a Pull Request

All contributors are expected to follow our [Code of Conduct](./CODE_OF_CONDUCT.md).

---

## 🔒 Security

If you discover a security vulnerability, please **do not** open a public GitHub issue. Instead, follow the instructions in [SECURITY.md](./SECURITY.md).

---

## 📝 Changelog

See [CHANGELOG.md](./CHANGELOG.md) for a full history of releases and changes.

---

## 📄 License

Distributed under the **MIT License**. See [LICENSE](./LICENSE) for more information.

---

<div align="center">

Made with ❤️ by the EcoSwitch AI team · [⭐ Star this repo](https://github.com/YOUR_USERNAME/ecoswitch-ai) if you find it useful!

</div>
