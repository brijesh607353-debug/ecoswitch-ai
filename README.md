<div align="center">

# ⚡ EcoSwitch AI

**Smart Energy Waste Reduction System**<br/>
*Built for the vivo Ignite Innovation Challenge 2026*

[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-24-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-5-000000?logo=express&logoColor=white)](https://expressjs.com/)
[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)](https://react.dev/)
[![Vite](https://img.shields.io/badge/Vite-6-646CFF?logo=vite&logoColor=white)](https://vitejs.dev/)
[![pnpm](https://img.shields.io/badge/pnpm-workspace-F69220?logo=pnpm&logoColor=white)](https://pnpm.io/)
[![Firebase](https://img.shields.io/badge/Firebase-planned-FFCA28?logo=firebase&logoColor=black)](#firebase-setup)
[![PWA](https://img.shields.io/badge/PWA-planned-5A0FC8)](#pwa-features)
[![ESP32](https://img.shields.io/badge/ESP32-planned-E7352C?logo=espressif&logoColor=white)](#iot-architecture)
[![License: MIT](https://img.shields.io/badge/License-MIT-16a34a)](./LICENSE)
[![CI](https://img.shields.io/github/actions/workflow/status/brijesh607353-debug/ecoswitch-ai/ci.yml?label=CI)](https://github.com/brijesh607353-debug/ecoswitch-ai/actions)
[![Last Commit](https://img.shields.io/github/last-commit/brijesh607353-debug/ecoswitch-ai)](https://github.com/brijesh607353-debug/ecoswitch-ai/commits/main)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen)](./CONTRIBUTING.md)

<br/>

> **Status: Active Development** — API layer, codegen pipeline, and database scaffolding are complete. Dashboard, Firebase, IoT, and PWA are next.

<br/>

[📖 Docs](./docs/) · [🏗 Architecture](./docs/ARCHITECTURE.md) · [📡 API](./docs/API.md) · [🗺 Roadmap](./docs/ROADMAP.md) · [🐛 Issues](https://github.com/brijesh607353-debug/ecoswitch-ai/issues) · [💬 Discussions](https://github.com/brijesh607353-debug/ecoswitch-ai/discussions)

</div>

---

## Table of Contents

- [About](#about)
- [Screenshots](#screenshots)
- [Architecture](#architecture)
- [Implementation Status](#implementation-status)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Troubleshooting](#troubleshooting)
- [Roadmap](#roadmap)
- [FAQ](#faq)
- [Contributing](#contributing)
- [Acknowledgements](#acknowledgements)
- [License](#license)

---

## About

**EcoSwitch AI** monitors household and business energy consumption through IoT sensors (ESP32), surfaces AI-driven insights to reduce waste, and helps users switch to greener energy plans — all from a single real-time dashboard.

The project follows a **contract-first API design**: every endpoint is defined in an OpenAPI 3.1 spec before it is implemented, automatically generating typed React Query hooks and Zod validation schemas across the full stack. No manual type duplication.

---

## Screenshots

> Real screenshots will be added as features are implemented. SVG diagrams below show the planned UI and system structure.

| System Overview | API Architecture |
|:-:|:-:|
| ![System Overview](./screenshots/system-overview.svg) | ![Architecture](./screenshots/architecture.svg) |

| Dashboard (Planned) | Hardware Layer (Planned) |
|:-:|:-:|
| ![Dashboard](./screenshots/dashboard-placeholder.svg) | ![Hardware](./screenshots/hardware.svg) |

---

## Architecture

### System Architecture

```mermaid
flowchart TB
    subgraph iot["🔌 IoT Layer · Planned"]
        SEN["Current + Voltage Sensors\nSCT-013 · ZMPT101B"]
        ESP["ESP32 MCU"]
        SEN --> ESP
    end

    subgraph backend["⚡ Backend · Implemented"]
        API["Express 5 REST API"]
        ZOD["Zod Validation"]
        LOG["Pino Logger"]
        API --> ZOD
        API --> LOG
    end

    subgraph data["🗄️ Data Layer · Scaffolded"]
        ORM["Drizzle ORM"]
        PG[("PostgreSQL")]
        FB[("Firebase Firestore\nPlanned")]
        ORM --> PG
    end

    subgraph frontend["🖥️ Frontend · In Development"]
        REACT["React 19 + Vite 6"]
        UI["shadcn/ui · 50+ components"]
        RQ["TanStack React Query"]
        CHARTS["Recharts · Planned"]
    end

    ESP -->|"POST /api/readings · Planned"| API
    REACT -->|"Generated React Query hooks"| API
    ZOD --> ORM
    API -.->|"Planned"| FB
```

### API Request Flow

```mermaid
sequenceDiagram
    participant C as React Client
    participant A as Express API
    participant Z as Zod Schema
    participant D as Drizzle / PostgreSQL

    C->>A: GET /api/healthz
    A->>Z: validate response shape
    Z-->>A: HealthStatus { status }
    A-->>C: 200 { "status": "ok" }

    Note over C,D: Future authenticated flow (planned)
    C->>A: POST /api/readings + Bearer token
    A->>Z: validate sensor payload
    Z->>D: insert reading
    D-->>A: confirmed
    A-->>C: 201 Created
```

### Development Data Flow

```mermaid
flowchart LR
    SPEC["lib/api-spec/\nopenapi.yaml"]
    GEN["Orval codegen"]
    HOOKS["lib/api-client-react/\nReact Query hooks"]
    SCHEMAS["lib/api-zod/\nZod schemas + types"]
    SERVER["artifacts/api-server/\nExpress route handlers"]
    CLIENT["artifacts/mockup-sandbox/\nReact components"]

    SPEC --> GEN
    GEN --> HOOKS
    GEN --> SCHEMAS
    SCHEMAS --> SERVER
    HOOKS --> CLIENT
```

---

## Implementation Status

| Feature | Status | Since |
|---------|:------:|-------|
| pnpm monorepo workspace | ✅ Implemented | v0.1.0 |
| Express 5 REST API server | ✅ Implemented | v0.1.0 |
| OpenAPI 3.1 contract (`lib/api-spec/openapi.yaml`) | ✅ Implemented | v0.1.0 |
| Orval codegen → React Query hooks + Zod schemas | ✅ Implemented | v0.1.0 |
| PostgreSQL + Drizzle ORM (connection layer) | ✅ Implemented | v0.1.0 |
| `GET /api/healthz` endpoint | ✅ Implemented | v0.1.0 |
| Pino structured logging | ✅ Implemented | v0.1.0 |
| 50+ shadcn/ui components (sandbox) | ✅ Implemented | v0.1.0 |
| Client-side auth token hook (stub) | ✅ Implemented | v0.1.0 |
| GitHub Actions CI (typecheck + prettier) | ✅ Implemented | v0.1.0 |
| Database schema (tables) | 🚧 In Progress | v0.2.0 |
| Device + user CRUD endpoints | 🚧 In Progress | v0.2.0 |
| Firebase Authentication | 📌 Planned | v0.3.0 |
| Energy consumption dashboard UI | 📌 Planned | v0.4.0 |
| IoT sensor ingestion endpoint | 📌 Planned | v0.5.0 |
| ESP32 firmware | 📌 Planned | v0.5.0 |
| Analytics & reporting | 📌 Planned | v0.6.0 |
| PDF report export | 📌 Planned | v0.6.0 |
| Push notifications (FCM) | 📌 Planned | v0.6.0 |
| PWA + Service Worker | 📌 Planned | v0.7.0 |
| Offline support | 📌 Planned | v0.7.0 |

---

## Tech Stack

| Layer | Technology | Version | Status |
|-------|-----------|---------|--------|
| Runtime | Node.js | 24 | ✅ |
| Language | TypeScript (strict) | 5.9 | ✅ |
| API Framework | Express | 5 | ✅ |
| Database | PostgreSQL + Drizzle ORM | — | ✅ Scaffolded |
| Validation | Zod + drizzle-zod | v4 | ✅ |
| API Contract | OpenAPI 3.1 + Orval | — | ✅ |
| Frontend | React + Vite | 19 + 6 | ✅ Sandbox |
| UI Components | shadcn/ui + Radix UI | — | ✅ |
| Data Fetching | TanStack React Query | v5 | ✅ |
| Logging | Pino | — | ✅ |
| Package Manager | pnpm workspaces | 9 | ✅ |
| Auth | Firebase Auth | — | 📌 Planned |
| Real-time DB | Firebase Firestore | — | 📌 Planned |
| IoT | ESP32 + sensors | — | 📌 Planned |
| Charts | Recharts | — | 📌 Installed |
| Animation | Framer Motion | — | 📌 Installed |
| PWA | Vite PWA Plugin + Workbox | — | 📌 Planned |

---

## Project Structure

```
ecoswitch-ai/
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.yml
│   │   ├── feature_request.yml
│   │   └── config.yml
│   ├── workflows/
│   │   └── ci.yml                  # TypeScript + Prettier CI
│   ├── CODEOWNERS
│   ├── dependabot.yml
│   └── PULL_REQUEST_TEMPLATE.md
│
├── artifacts/
│   ├── api-server/                  # ⚡ Express 5 REST API
│   │   └── src/
│   │       ├── app.ts               # Express setup (CORS, logging, routing)
│   │       ├── index.ts             # Entry point — binds to $PORT
│   │       ├── routes/
│   │       │   ├── index.ts         # Route aggregator
│   │       │   └── health.ts        # GET /api/healthz
│   │       ├── middlewares/         # Auth, rate-limit (planned)
│   │       └── lib/logger.ts        # Pino singleton
│   │
│   └── mockup-sandbox/              # 🖥️ React + Vite UI sandbox (dev only)
│       └── src/
│           ├── components/ui/       # 50+ shadcn/ui components
│           └── hooks/               # use-toast, use-mobile
│
├── lib/
│   ├── api-spec/
│   │   └── openapi.yaml            # ← Source of truth for all API contracts
│   ├── api-client-react/           # Generated — React Query hooks (do not edit)
│   ├── api-zod/                    # Generated — Zod schemas + TS types (do not edit)
│   └── db/
│       ├── src/schema/             # Drizzle table definitions (empty — v0.2.0)
│       └── drizzle.config.ts
│
├── docs/                           # Extended documentation
│   ├── ARCHITECTURE.md
│   ├── API.md
│   ├── DEVELOPMENT.md
│   ├── DEPLOYMENT.md
│   ├── FIREBASE.md
│   ├── IOT.md
│   ├── PWA.md
│   ├── ROADMAP.md
│   ├── TESTING.md
│   ├── PERFORMANCE.md
│   ├── KNOWN_LIMITATIONS.md
│   └── GITHUB_LABELS.md
│
├── screenshots/                    # App screenshots and SVG diagrams
├── scripts/                        # Utility scripts
│
├── .env.example                    # All required env vars (no secrets)
├── .gitignore                      # Node, Next.js, Firebase, pnpm, Replit
├── CHANGELOG.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE                         # MIT
├── SECURITY.md
├── pnpm-workspace.yaml             # Workspace packages + dependency catalog
└── tsconfig.base.json              # Shared strict TypeScript config
```

---

## Getting Started

### Prerequisites

| Tool | Version | Install |
|------|---------|---------|
| Node.js | ≥ 20 | [nodejs.org](https://nodejs.org/) |
| pnpm | ≥ 9 | `npm install -g pnpm` |
| PostgreSQL | ≥ 15 | [postgresql.org](https://www.postgresql.org/download/) |

### 1. Clone and Install

```bash
git clone https://github.com/brijesh607353-debug/ecoswitch-ai.git
cd ecoswitch-ai
pnpm install
```

### 2. Configure Environment

```bash
cp .env.example .env.local
```

Minimum required values for local development:

```env
DATABASE_URL=postgresql://postgres:password@localhost:5432/ecoswitch
SESSION_SECRET=your-random-secret-here
```

Generate a secure session secret:

```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

### 3. Push Database Schema

```bash
pnpm --filter @workspace/db run push
```

> Note: the schema is currently empty (v0.1.0). This creates the connection and verifies `DATABASE_URL` is valid.

### 4. Start the API Server

```bash
pnpm --filter @workspace/api-server run dev
```

### 5. Verify

```bash
curl http://localhost:5000/api/healthz
# {"status":"ok"}
```

---

## Development Workflow

### API Changes (contract-first)

```bash
# 1. Edit the OpenAPI spec — this is the source of truth
$EDITOR lib/api-spec/openapi.yaml

# 2. Regenerate React Query hooks and Zod schemas
pnpm --filter @workspace/api-spec run codegen

# 3. Implement the Express route handler (schemas are now available)
# 4. Wire the hook in the frontend
```

> **Do not edit** `lib/api-client-react/src/generated/` or `lib/api-zod/src/generated/` directly — they are overwritten on every codegen run.

### Database Schema Changes

```bash
# 1. Add or edit a table in lib/db/src/schema/
# 2. Push to your local database (development only)
pnpm --filter @workspace/db run push

# 3. Generate a migration file (for production deployments)
pnpm --filter @workspace/db run generate
```

### TypeScript Check

```bash
# Full check across all packages (run before every commit)
pnpm run typecheck
```

### Formatting

```bash
# Format all files
pnpm prettier --write .

# Check only (used in CI)
pnpm prettier --check .
```

### Build

```bash
# Build all packages (typecheck + esbuild bundle)
pnpm run build
```

---

## Troubleshooting

### `DATABASE_URL` connection error on startup

```
Error: connect ECONNREFUSED 127.0.0.1:5432
```

**Fix:** PostgreSQL is not running. Start it with:
```bash
# macOS (Homebrew)
brew services start postgresql@15

# Ubuntu/Debian
sudo systemctl start postgresql

# Windows
pg_ctl -D "C:\Program Files\PostgreSQL\15\data" start
```

---

### Port already in use

```
Error: listen EADDRINUSE: address already in use :::5000
```

**Fix:** Set a different port:
```bash
PORT=5001 pnpm --filter @workspace/api-server run dev
```

---

### Codegen produces no output or empty files

**Cause:** The OpenAPI spec has a syntax error.

**Fix:** Validate the spec first:
```bash
npx @redocly/cli lint lib/api-spec/openapi.yaml
```

---

### TypeScript errors after codegen

**Cause:** Stale compiled lib declarations.

**Fix:**
```bash
pnpm run typecheck:libs  # rebuilds lib/* declarations
pnpm run typecheck       # full check
```

---

### `pnpm install` fails with frozen lockfile error

**Fix:** You likely have uncommitted lockfile changes. Either commit them or run:
```bash
pnpm install --no-frozen-lockfile
```
Then commit the updated `pnpm-lock.yaml`.

---

## Roadmap

See [docs/ROADMAP.md](./docs/ROADMAP.md) for the detailed milestone breakdown.

```
v0.1.0  ✅  Monorepo · Express API · OpenAPI pipeline · DB scaffolding · CI
v0.2.0  🚧  Database schema · Device/user/reading tables · CRUD endpoints
v0.3.0  📌  Firebase Auth · Protected routes · Session management
v0.4.0  📌  Energy dashboard · Recharts integration · Real-time data
v0.5.0  📌  IoT ingestion endpoint · ESP32 firmware · Sensor simulator
v0.6.0  📌  Analytics · PDF reports · Push notifications (FCM)
v0.7.0  📌  PWA · Service Worker · Offline support · Installable
v1.0.0  📌  Production-ready · Performance-tuned · Full test coverage
```

---

## FAQ

**Q: Why Express 5 instead of Next.js API routes?**  
A: The monorepo keeps frontend and backend independently deployable. Express 5 gives full control over middleware, logging, and the API contract. Next.js is referenced in `.env.example` variable names only — the project is Vite + Express.

**Q: Why OpenAPI-first / codegen instead of tRPC or GraphQL?**  
A: OpenAPI produces a language-agnostic contract that the ESP32 firmware and any future mobile app can consume. tRPC ties you to TypeScript on both ends; GraphQL adds runtime overhead for a REST IoT use-case.

**Q: Is Firebase required to run locally?**  
A: No. Firebase is planned for v0.3.0. All Firebase environment variables in `.env.example` are optional for local development.

**Q: Why pnpm workspaces?**  
A: Monorepo tooling lets shared code (`lib/db`, `lib/api-zod`) be consumed by multiple packages without publishing to npm, while keeping each package independently typed and buildable.

**Q: Can I contribute even if ESP32 hardware is not implemented yet?**  
A: Yes — most open work is in the API, database schema, and dashboard (no hardware needed). See issues labelled [`good first issue`](https://github.com/brijesh607353-debug/ecoswitch-ai/labels/good%20first%20issue).

**Q: Why Drizzle ORM over Prisma?**  
A: Drizzle is TypeScript-native, produces no generated client at runtime, and integrates directly with `drizzle-zod` for schema-derived Zod types — keeping the validation pipeline fully unified.

---

## Contributing

Read [CONTRIBUTING.md](./CONTRIBUTING.md) for the full development setup, branch naming, commit conventions (Conventional Commits), and PR process.

All contributors are expected to follow the [Code of Conduct](./CODE_OF_CONDUCT.md).

Quick start:

```bash
# Fork the repo, then:
git clone https://github.com/<your-username>/ecoswitch-ai.git
git checkout -b feat/your-feature
# make changes
pnpm run typecheck
git commit -m "feat: describe your change"
git push origin feat/your-feature
# open a Pull Request
```

---

## Acknowledgements

| Tool / Project | Role in EcoSwitch AI |
|---------------|----------------------|
| [Express](https://expressjs.com/) | REST API framework |
| [Drizzle ORM](https://orm.drizzle.team/) | Type-safe database access |
| [Orval](https://orval.dev/) | OpenAPI → React Query codegen |
| [shadcn/ui](https://ui.shadcn.com/) | Component library (50+ components) |
| [TanStack Query](https://tanstack.com/query) | Server state management |
| [Zod](https://zod.dev/) | Runtime schema validation |
| [Pino](https://getpino.io/) | High-performance structured logging |
| [Vite](https://vitejs.dev/) | Frontend build tool |
| [Radix UI](https://www.radix-ui.com/) | Accessible component primitives |
| [vivo Ignite Challenge](https://www.vivo.com/) | Competition motivating this project |

---

## License

MIT © 2026 [EcoSwitch AI Contributors](https://github.com/brijesh607353-debug/ecoswitch-ai/graphs/contributors)

See [LICENSE](./LICENSE) for the full text.

---

<div align="center">

Built for the **vivo Ignite Innovation Challenge 2026**

[⭐ Star this repo](https://github.com/brijesh607353-debug/ecoswitch-ai) · [🐛 Open an Issue](https://github.com/brijesh607353-debug/ecoswitch-ai/issues) · [💬 Start a Discussion](https://github.com/brijesh607353-debug/ecoswitch-ai/discussions)

</div>
