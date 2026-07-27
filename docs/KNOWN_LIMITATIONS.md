# Known Limitations — EcoSwitch AI

> This document is the honest record of what does not work, is not implemented, or has known issues. It is updated with every release.

---

## Current Limitations (v0.1.0)

### No Database Tables

**What it means:** The Drizzle ORM connection is configured and the schema file exists, but no tables have been defined. Calling any route that queries the database will fail.

**When it will be fixed:** v0.2.0 — device, user, and energy-reading tables are the next milestone.

**Workaround:** None — avoid database queries until schema is defined.

---

### No Authentication

**What it means:** All API endpoints are currently unauthenticated. Anyone with network access to the API can call any endpoint.

**When it will be fixed:** v0.3.0 — Firebase Authentication middleware will be added.

**Workaround:** Do not deploy to a public URL until authentication is in place. For local development this is acceptable.

---

### Firebase Not Connected

**What it means:** `.env.example` lists Firebase environment variables and `CHANGELOG.md` mentions Firebase as a planned feature, but no Firebase SDK code exists in the codebase.

**When it will be fixed:** v0.3.0.

**Workaround:** N/A — Firebase integration is planned, not yet started.

---

### No IoT Hardware Tested

**What it means:** The ESP32 integration is in design phase only. No firmware has been written, no circuits have been tested, no real sensor readings have been ingested.

**When it will be fixed:** v0.5.0 — requires hardware procurement and firmware development.

**Workaround:** A mock simulator script (`scripts/simulate-readings.ts`) is planned for testing without hardware.

---

### PWA Not Implemented

**What it means:** There is no Service Worker, no Web App Manifest, and no offline support. The app cannot be installed or used offline.

**When it will be fixed:** v0.7.0.

**Workaround:** N/A.

---

### UI Sandbox Is Development-Only

**What it means:** `artifacts/mockup-sandbox` is a component preview tool for design iteration. It is not the production frontend — it has no routing, no real data connections, and no authentication.

**When it will be fixed:** The production frontend will be built as a separate React/Vite artifact in a future milestone.

**Workaround:** N/A — this is by design.

---

### No Tests

**What it means:** There are no unit, integration, or end-to-end tests. The only automated quality gate is the TypeScript typecheck.

**When it will be fixed:** Tests will be introduced alongside the first real business-logic code (database schema + route handlers in v0.2.0).

**Workaround:** Run `pnpm run typecheck` before every commit.

---

### Next.js Referenced But Not Used

**What it means:** `.env.example` uses `NEXT_PUBLIC_` prefix (a Next.js convention) and `package.json` references `next-themes`. The project is built on **Vite + Express**, not Next.js. These are carryovers from the initial scaffolding that will be cleaned up.

**When it will be fixed:** When the production frontend is defined, the correct prefix (`VITE_`) will be used.

---

## Reporting New Limitations

If you discover a limitation not listed here, please open an issue with the `limitation` label or submit a PR updating this file.
