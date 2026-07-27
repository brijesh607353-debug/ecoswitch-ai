<div align="center">

<img src="./screenshots/logo-placeholder.png" alt="EcoSwitch AI Logo" width="120" />

# ⚡ EcoSwitch AI

### Smart Energy Waste Reduction System

**Built for the vivo Ignite Innovation Challenge 2026**

[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-24-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-5-000000?logo=express&logoColor=white)](https://expressjs.com/)
[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)](https://react.dev/)
[![Vite](https://img.shields.io/badge/Vite-6-646CFF?logo=vite&logoColor=white)](https://vitejs.dev/)
[![Firebase](https://img.shields.io/badge/Firebase-planned-FFCA28?logo=firebase&logoColor=black)](#firebase-setup)
[![PWA](https://img.shields.io/badge/PWA-planned-5A0FC8?logo=pwa&logoColor=white)](#pwa-features)
[![ESP32](https://img.shields.io/badge/ESP32-planned-E7352C?logo=espressif&logoColor=white)](#iot-architecture)
[![License: MIT](https://img.shields.io/badge/License-MIT-16a34a?logo=opensourceinitiative&logoColor=white)](./LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./CONTRIBUTING.md)
[![Last Commit](https://img.shields.io/github/last-commit/brijesh607353-debug/ecoswitch-ai)](https://github.com/brijesh607353-debug/ecoswitch-ai/commits/main)
[![GitHub Stars](https://img.shields.io/github/stars/brijesh607353-debug/ecoswitch-ai?style=social)](https://github.com/brijesh607353-debug/ecoswitch-ai)

<br />

> **⚠️ Status: Active Development** — Core API and data layer are in place. Dashboard, Firebase integration, IoT layer, and PWA features are under active development.

<br />

[📖 Architecture](./docs/ARCHITECTURE.md) · [🔥 Firebase Guide](./docs/FIREBASE.md) · [🔌 IoT Guide](./docs/IOT.md) · [📡 API Reference](./docs/API.md) · [🐛 Report Bug](https://github.com/brijesh607353-debug/ecoswitch-ai/issues/new?template=bug_report.yml) · [✨ Request Feature](https://github.com/brijesh607353-debug/ecoswitch-ai/issues/new?template=feature_request.yml)

</div>

---

## 📋 Table of Contents

- [About the Project](#about-the-project)
- [Implementation Status](#implementation-status)
- [Architecture Overview](#architecture-overview)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [API Reference](#api-reference)
- [Firebase Setup](#firebase-setup)
- [IoT Architecture](#iot-architecture)
- [PWA Features](#pwa-features)
- [Dashboard Features](#dashboard-features)
- [Roadmap](#roadmap)
- [Known Limitations](#known-limitations)
- [Contributing](#contributing)
- [Security](#security)
- [License](#license)

---

## 🌍 About the Project

**EcoSwitch AI** is a smart energy waste reduction system designed to help households and businesses monitor, analyse, and reduce their energy consumption. By combining an IoT sensor layer (ESP32), a real-time cloud backend (Firebase), and an AI-driven recommendation engine, EcoSwitch AI surfaces actionable insights that lead to lower bills and a smaller carbon footprint.

Submitted to the **vivo Ignite Innovation Challenge 2026** — a competition recognising innovative, impact-driven software engineering projects.

---

## ✅ Implementation Status

This section is the single source of truth for what is and is not in the codebase. It is updated with every release.

### Implemented

| Feature | Status | Notes |
|---------|--------|-------|
| pnpm monorepo workspace | ✅ Complete | `artifacts/`, `lib/`, `scripts/` |
| Express 5 REST API server | ✅ Complete | TypeScript strict, Pino logging |
| OpenAPI 3.1 contract | ✅ Complete | `lib/api-spec/openapi.yaml` |
| Orval codegen pipeline | ✅ Complete | Generates React Query hooks + Zod schemas |
| React Query client hooks | ✅ Complete | Auto-generated from OpenAPI spec |
| Zod validation schemas | ✅ Complete | Auto-generated from OpenAPI spec |
| PostgreSQL + Drizzle ORM | ✅ Scaffolded | Connection layer ready; tables not yet defined |
| React + Vite UI sandbox | ✅ Complete | 50+ shadcn/ui components available |
| `GET /api/healthz` endpoint | ✅ Complete | Returns `{ status: "ok" }` |
| Structured logging (Pino) | ✅ Complete | JSON in production, pretty-print in dev |
| Client-side auth token hook | ✅ Stub | `setAuthTokenGetter` in `custom-fetch.ts` |

### Planned / In Active Development

| Feature | Status | Blocker |
|---------|--------|---------|
| Database schema (tables) | 🔨 In design | None — next milestone |
| Firebase Authentication | 🔨 Planned | Firebase project setup required |
| Firebase Firestore | 🔨 Planned | Firebase project setup required |
| Energy consumption dashboard | 🔨 Planned | Requires DB schema + Firebase |
| Real-time analytics | 🔨 Planned | Requires dashboard |
| PDF report export | 🔨 Planned | Requires dashboard |
| Push notifications | 🔨 Planned | Requires Firebase Cloud Messaging |
| Search functionality | 🔨 Planned | Requires data layer |
| IoT integration (ESP32) | 🔨 Planned | Requires hardware + firmware |
| PWA / Service Worker | 🔨 Planned | Requires frontend build |
| Offline support | 🔨 Planned | Requires PWA layer |

> **Integrity note:** Features are only moved to "Implemented" when the code is merged to `main`. No feature is claimed as working unless it is verifiable in this repository.

---

## 🏗 Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        EcoSwitch AI                         │
│                                                             │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│  │   IoT Layer  │    │  API Layer   │    │  UI Layer    │  │
│  │  (Planned)   │───▶│  (Express 5) │◀───│ (React+Vite) │  │
│  │   ESP32 +    │    │  TypeScript  │    │  shadcn/ui   │  │
│  │   Sensors    │    │  Zod + Pino  │    │  React Query │  │
│  └──────────────┘    └──────┬───────┘    └──────────────┘  │
│                             │                               │
│                      ┌──────▼───────┐                       │
│                      │  Data Layer  │                       │
│                      │  PostgreSQL  │                       │
│                      │  Drizzle ORM │                       │
│                      │  (Firebase   │                       │
│                      │   planned)   │                       │
│                      └──────────────┘                       │
└─────────────────────────────────────────────────────────────┘
```

The API follows a **contract-first** design: every endpoint is defined in the OpenAPI spec (`lib/api-spec/openapi.yaml`) before implementation. This single source of truth drives automatic generation of typed React Query hooks and Zod validation schemas across the entire stack.

See [docs/ARCHITECTURE.md](./docs/ARCHITECTURE.md) for a detailed breakdown.

---

## 🛠 Technology Stack

| Layer | Technology | Status |
|-------|-----------|--------|
| **Runtime** | Node.js 24 | ✅ Active |
| **Language** | TypeScript 5.9 (strict) | ✅ Active |
| **API Framework** | Express 5 | ✅ Active |
| **Database** | PostgreSQL + Drizzle ORM | ✅ Scaffolded |
| **Validation** | Zod v4 + drizzle-zod | ✅ Active |
| **API Contract** | OpenAPI 3.1 + Orval | ✅ Active |
| **Frontend** | React 19 + Vite 6 | ✅ Active |
| **UI Components** | shadcn/ui + Radix UI | ✅ Active |
| **State / Data** | TanStack React Query v5 | ✅ Active |
| **Logging** | Pino | ✅ Active |
| **Package Manager** | pnpm workspaces | ✅ Active |
| **Authentication** | Firebase Auth | 🔨 Planned |
| **Real-time DB** | Firebase Firestore | 🔨 Planned |
| **IoT** | ESP32 + Arduino/MicroPython | 🔨 Planned |
| **PWA** | Service Worker + Web App Manifest | 🔨 Planned |
| **Charts** | Recharts | 🔨 Installed, not yet wired |
| **Animation** | Framer Motion | 🔨 Installed, not yet wired |

---

## 📁 Project Structure

```
ecoswitch-ai/
├── artifacts/
│   ├── api-server/              # Express 5 REST API
│   │   └── src/
│   │       ├── app.ts           # Express app setup (CORS, logging, routing)
│   │       ├── index.ts         # Server entry point
│   │       ├── routes/
│   │       │   ├── index.ts     # Route aggregator
│   │       │   └── health.ts    # GET /api/healthz
│   │       ├── middlewares/     # (empty — ready for auth middleware)
│   │       └── lib/
│   │           └── logger.ts    # Pino logger singleton
│   └── mockup-sandbox/          # React + Vite UI component sandbox
│       └── src/
│           ├── components/ui/   # 50+ shadcn/ui components
│           └── hooks/           # use-toast, use-mobile
├── lib/
│   ├── api-spec/
│   │   └── openapi.yaml         # ← OpenAPI source of truth
│   ├── api-client-react/        # Generated: React Query hooks
│   ├── api-zod/                 # Generated: Zod schemas + TypeScript types
│   └── db/
│       ├── src/schema/          # Drizzle schema (currently empty)
│       └── drizzle.config.ts    # Drizzle Kit config
├── scripts/                     # Utility scripts
├── docs/                        # Extended documentation
│   ├── ARCHITECTURE.md
│   ├── API.md
│   ├── FIREBASE.md
│   ├── IOT.md
│   ├── PWA.md
│   ├── DEPLOYMENT.md
│   ├── TESTING.md
│   ├── PERFORMANCE.md
│   └── KNOWN_LIMITATIONS.md
├── screenshots/                 # App screenshots
├── .env.example                 # Required environment variables (no secrets)
├── pnpm-workspace.yaml          # Workspace packages + dependency catalog
└── tsconfig.base.json           # Shared strict TypeScript configuration
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** ≥ 20 ([download](https://nodejs.org/))
- **pnpm** ≥ 9 — `npm install -g pnpm`
- **PostgreSQL** ≥ 15 ([download](https://www.postgresql.org/download/))

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/brijesh607353-debug/ecoswitch-ai.git
cd ecoswitch-ai

# 2. Install all workspace dependencies
pnpm install

# 3. Set up environment variables
cp .env.example .env.local
# Fill in your values — see Environment Variables section
```

### Development

```bash
# Start the API server (port from $PORT, default 5000)
pnpm --filter @workspace/api-server run dev

# After editing lib/api-spec/openapi.yaml — regenerate hooks & schemas
pnpm --filter @workspace/api-spec run codegen

# Push DB schema changes to your local database (development only)
pnpm --filter @workspace/db run push

# Full TypeScript check across all packages
pnpm run typecheck

# Build all packages
pnpm run build
```

### Verify the API is running

```bash
curl http://localhost:5000/api/healthz
# → {"status":"ok"}
```

---

## 🔐 Environment Variables

Copy `.env.example` to `.env.local` and fill in your values. See [`.env.example`](./.env.example) for descriptions.

| Variable | Required | Description |
|----------|----------|-------------|
| `DATABASE_URL` | ✅ | PostgreSQL connection string |
| `SESSION_SECRET` | ✅ | Random secret for session signing |
| `PORT` | Optional | API server port (default: 5000) |
| `NODE_ENV` | Optional | `development` / `production` |
| `NEXT_PUBLIC_FIREBASE_*` | 🔨 Planned | Firebase project configuration |
| `FIREBASE_ADMIN_CREDENTIAL` | 🔨 Planned | Firebase Admin SDK (server-side) |
| `OPENAI_API_KEY` | 🔨 Planned | AI recommendation engine |

> **Never commit `.env.local`** — it is git-ignored. Rotate any credential immediately if accidentally exposed.

---

## 📡 API Reference

The full contract lives in [`lib/api-spec/openapi.yaml`](./lib/api-spec/openapi.yaml). See [docs/API.md](./docs/API.md) for detailed usage examples.

### Implemented Endpoints

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `GET` | `/api/healthz` | None | Server health check |

### Planned Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/auth/register` | User registration |
| `POST` | `/api/auth/login` | User login |
| `GET` | `/api/devices` | List connected IoT devices |
| `GET` | `/api/energy` | Energy consumption data |
| `GET` | `/api/analytics` | Usage analytics and trends |
| `POST` | `/api/reports` | Generate PDF report |

---

## 🔥 Firebase Setup

> **Status: Planned** — Firebase integration is not yet implemented. The environment variable placeholders and project structure are ready.

See [docs/FIREBASE.md](./docs/FIREBASE.md) for the planned integration guide.

---

## 🔌 IoT Architecture

> **Status: Planned** — Requires ESP32 hardware and firmware development.

See [docs/IOT.md](./docs/IOT.md) for the planned ESP32 integration architecture.

---

## 📱 PWA Features

> **Status: Planned** — Progressive Web App features including offline support and installability.

See [docs/PWA.md](./docs/PWA.md) for the planned PWA implementation guide.

---

## 📊 Dashboard Features

> **Status: Planned** — The UI component library (shadcn/ui, Recharts, Framer Motion) is installed and available; the dashboard is not yet implemented.

Planned dashboard capabilities:
- Real-time energy consumption charts
- Device-level breakdown
- Cost vs. CO₂ toggle
- Historical trend comparisons
- Anomaly alerts

---

## 🗺 Roadmap

```
v0.1.0  ✅  Monorepo scaffold, Express API, OpenAPI pipeline, DB layer
v0.2.0  🔨  Database schema + CRUD endpoints for devices and readings
v0.3.0  🔨  Firebase Auth + user management
v0.4.0  🔨  Energy dashboard UI (Recharts)
v0.5.0  🔨  IoT ingestion endpoint + ESP32 firmware
v0.6.0  🔨  Analytics, reports, PDF export
v0.7.0  🔨  PWA + Service Worker + offline support
v1.0.0  🔨  Production-ready release
```

---

## ⚠️ Known Limitations

See [docs/KNOWN_LIMITATIONS.md](./docs/KNOWN_LIMITATIONS.md) for the full list. Key points:

- **No database tables yet** — Drizzle schema is scaffolded but empty
- **No authentication** — API endpoints are currently unauthenticated
- **No IoT hardware tested** — ESP32 integration is in design phase
- **Firebase not connected** — Only env var placeholders exist
- **PWA not implemented** — No service worker or web app manifest

---

## 🤝 Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for development setup, branch naming, commit conventions, and the PR process.

All contributors must follow our [Code of Conduct](./CODE_OF_CONDUCT.md).

---

## 🔒 Security

Report vulnerabilities privately — see [SECURITY.md](./SECURITY.md). Do not open public issues for security bugs.

---

## 📝 Changelog

Full history in [CHANGELOG.md](./CHANGELOG.md).

---

## 📄 License

MIT License — see [LICENSE](./LICENSE).

---

<div align="center">

**EcoSwitch AI** — Built for the vivo Ignite Innovation Challenge 2026

[⭐ Star this repo](https://github.com/brijesh607353-debug/ecoswitch-ai) · [🐛 Open an Issue](https://github.com/brijesh607353-debug/ecoswitch-ai/issues) · [💬 Discussions](https://github.com/brijesh607353-debug/ecoswitch-ai/discussions)

</div>
