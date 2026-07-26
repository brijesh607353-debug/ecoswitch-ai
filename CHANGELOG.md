# Changelog

All notable changes to **EcoSwitch AI** are documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Planned
- AI-powered energy plan recommendation engine
- User authentication and profile management
- Energy usage dashboard with real-time data
- Carbon footprint calculator
- Supplier API integrations

---

## [0.1.0] — 2026-07-26

### Added
- Initial project scaffolding with pnpm monorepo structure
- Express 5 API server with TypeScript strict mode (`artifacts/api-server`)
- Contract-first API design: OpenAPI 3.1 spec in `lib/api-spec/openapi.yaml`
- Auto-generated React Query hooks (`lib/api-client-react`) via Orval codegen
- Auto-generated Zod validation schemas (`lib/api-zod`) via Orval codegen
- PostgreSQL database layer with Drizzle ORM (`lib/db`)
- React + Vite component sandbox for UI development (`artifacts/mockup-sandbox`)
- Pino structured logging throughout the API server
- Health check endpoint: `GET /api/healthz`
- MIT License
- Comprehensive `.gitignore` covering Node.js, Next.js, Firebase, and pnpm
- `.env.example` with all required environment variable definitions
- `CONTRIBUTING.md` with development setup and commit conventions
- `CODE_OF_CONDUCT.md` (Contributor Covenant v2.1)
- `SECURITY.md` with responsible disclosure instructions
- `docs/` folder for extended documentation
- `screenshots/` folder for UI screenshots

---

[Unreleased]: https://github.com/YOUR_USERNAME/ecoswitch-ai/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/YOUR_USERNAME/ecoswitch-ai/releases/tag/v0.1.0
