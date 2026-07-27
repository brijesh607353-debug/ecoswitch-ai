# Changelog

All notable changes to **EcoSwitch AI** are documented here.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)
Versioning: [Semantic Versioning](https://semver.org/spec/v2.0.0.html)

---

## [Unreleased]

### In Active Development
- Database schema: device, user, and energy-reading tables (Drizzle)
- Firebase Authentication integration
- Energy consumption dashboard (Recharts)
- IoT ingestion endpoint for ESP32 sensor data
- Progressive Web App (Service Worker + manifest)

---

## [0.1.0] — 2026-07-27

Initial scaffolding commit for the EcoSwitch AI project.

### Added

**Monorepo infrastructure**
- pnpm workspace with `artifacts/`, `lib/`, and `scripts/` packages
- Root `tsconfig.base.json` with strict TypeScript defaults
- Root typecheck pipeline (`pnpm run typecheck`) covering all packages
- Shared dependency catalog in `pnpm-workspace.yaml`

**API Server** (`artifacts/api-server`)
- Express 5 REST API server with TypeScript strict mode
- Pino structured logging (JSON in production, pretty-print in dev)
- CORS and JSON body parsing middleware
- `GET /api/healthz` — health check endpoint returning `{ status: "ok" }`
- Zod schema validation using `@workspace/api-zod`

**API Contract** (`lib/api-spec`)
- OpenAPI 3.1 specification (`openapi.yaml`) — single source of truth
- Orval codegen pipeline generating React Query hooks and Zod schemas
- Auto-generated `@workspace/api-client-react` with `useHealthCheck` hook
- Auto-generated `@workspace/api-zod` with `HealthStatus` schema

**Data Layer** (`lib/db`)
- PostgreSQL connection via `drizzle-orm/node-postgres`
- Drizzle Kit config for schema migrations
- `drizzle-zod` for schema-derived Zod types
- Database schema scaffold (empty — tables in next release)

**UI Sandbox** (`artifacts/mockup-sandbox`)
- React 19 + Vite 6 component preview server
- 50+ shadcn/ui components (Radix UI primitives)
- TanStack React Query v5 client
- Recharts (charts library — not yet wired to data)
- Framer Motion (animation library — not yet wired)
- `next-themes` for dark/light mode support
- Dynamic component preview renderer for design iteration

**Client auth stub** (`lib/api-client-react`)
- `setAuthTokenGetter` in `custom-fetch.ts` — ready to wire Firebase Auth tokens

**Repository quality**
- MIT License
- Comprehensive `.gitignore` (Node.js, Next.js, Firebase, pnpm, Replit)
- `.env.example` with all required environment variables documented
- `CONTRIBUTING.md` with development setup and Conventional Commits guide
- `CODE_OF_CONDUCT.md` (Contributor Covenant v2.1)
- `SECURITY.md` with responsible disclosure instructions
- `docs/` folder with architecture, API, Firebase, IoT, PWA, deployment docs
- `screenshots/` folder with naming and contribution guide
- GitHub community files: issue templates, PR template, Dependabot, CI workflow

---

[Unreleased]: https://github.com/brijesh607353-debug/ecoswitch-ai/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/brijesh607353-debug/ecoswitch-ai/releases/tag/v0.1.0

