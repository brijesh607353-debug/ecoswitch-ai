# Architecture — EcoSwitch AI

> **Reflects the actual codebase as of v0.1.0.** Planned components are clearly labelled.

---

## Overview

EcoSwitch AI is a **pnpm monorepo** composed of independently deployable packages. Every API contract is defined once in an OpenAPI spec and propagated to the rest of the system via code generation — no manual type duplication.

```
┌─────────────────────────────────────────────────────────────────┐
│                         EcoSwitch AI                            │
│                                                                 │
│  ┌──────────────┐    ┌──────────────────┐    ┌──────────────┐  │
│  │  IoT Layer   │    │    API Layer      │    │   UI Layer   │  │
│  │  (Planned)   │───▶│  artifacts/       │◀───│ artifacts/   │  │
│  │  ESP32 +     │    │  api-server       │    │ mockup-      │  │
│  │  Sensors     │    │  Express 5 + Pino │    │ sandbox      │  │
│  └──────────────┘    └────────┬─────────┘    └──────────────┘  │
│                               │                                  │
│              ┌────────────────▼─────────────────┐               │
│              │           Data Layer              │               │
│              │  lib/db — PostgreSQL + Drizzle    │               │
│              │  (Firebase Firestore — planned)   │               │
│              └──────────────────────────────────┘               │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │               Shared Libraries (lib/)                    │    │
│  │  api-spec → openapi.yaml (source of truth)              │    │
│  │  api-client-react → generated React Query hooks         │    │
│  │  api-zod → generated Zod schemas + TS types             │    │
│  └─────────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────┘
```

---

## Monorepo Layout

| Package | Path | Role |
|---------|------|------|
| `@workspace/api-server` | `artifacts/api-server/` | Express 5 REST API — the only deployable backend |
| `@workspace/mockup-sandbox` | `artifacts/mockup-sandbox/` | React + Vite component sandbox (dev only) |
| `@workspace/api-spec` | `lib/api-spec/` | OpenAPI 3.1 spec + Orval codegen config |
| `@workspace/api-client-react` | `lib/api-client-react/` | **Generated** React Query hooks (do not edit manually) |
| `@workspace/api-zod` | `lib/api-zod/` | **Generated** Zod schemas + TypeScript types (do not edit manually) |
| `@workspace/db` | `lib/db/` | Drizzle ORM schema, migrations, and DB client |
| `@workspace/scripts` | `scripts/` | Utility scripts |

**Rules:**
- `artifacts/*` packages must never import from each other — shared logic lives in `lib/*`
- Generated packages (`api-client-react`, `api-zod`) must never be edited manually — regenerate with `pnpm --filter @workspace/api-spec run codegen`
- `lib/*` packages are composite TypeScript (they emit declarations); `artifacts/*` are leaf packages (they do not emit)

---

## Contract-First API Design

```
lib/api-spec/openapi.yaml          ← Edit here — single source of truth
          │
          ▼ pnpm --filter @workspace/api-spec run codegen
          │
    ┌─────┴─────┐
    │           │
    ▼           ▼
lib/api-client-react    lib/api-zod
(React Query hooks)     (Zod schemas + types)
```

**Workflow:**
1. Design the endpoint in `openapi.yaml` first
2. Run codegen — hooks and schemas appear automatically
3. Implement the Express route handler using the generated Zod schemas for validation
4. The frontend consumes the generated React Query hooks — no manual type copying

---

## API Server

**File:** `artifacts/api-server/src/`

| File | Purpose |
|------|---------|
| `index.ts` | Process entry point — binds to `process.env.PORT` |
| `app.ts` | Express app factory — registers middleware and router |
| `routes/index.ts` | Route aggregator — mounts all sub-routers |
| `routes/health.ts` | `GET /api/healthz` handler |
| `middlewares/` | Empty — ready for auth, rate-limiting middleware |
| `lib/logger.ts` | Pino singleton — use `req.log` in handlers, `logger` elsewhere |

**Key middleware stack (in order):**
1. `pino-http` — request/response logging
2. `cors` — cross-origin resource sharing
3. `express.json()` — JSON body parsing
4. `express.urlencoded()` — form body parsing
5. `/api` router — all application routes

---

## Data Layer

**File:** `lib/db/`

- **ORM:** Drizzle ORM with `drizzle-orm/node-postgres`
- **Migrations:** Drizzle Kit (`pnpm --filter @workspace/db run push` for dev)
- **Schema validation:** `drizzle-zod` generates Zod schemas from Drizzle table definitions
- **Current state:** Connection layer is configured; no tables are defined yet

**Adding a new table:**
```typescript
// lib/db/src/schema/devices.ts
import { pgTable, text, serial, timestamp } from "drizzle-orm/pg-core";
import { createInsertSchema } from "drizzle-zod";
import { z } from "zod/v4";

export const devicesTable = pgTable("devices", {
  id: serial("id").primaryKey(),
  name: text("name").notNull(),
  createdAt: timestamp("created_at").defaultNow().notNull(),
});

export const insertDeviceSchema = createInsertSchema(devicesTable).omit({ id: true });
export type InsertDevice = z.infer<typeof insertDeviceSchema>;
export type Device = typeof devicesTable.$inferSelect;
```

---

## Planned Architecture Additions

### Firebase (v0.3.0)
- Firebase Auth → replace the `setAuthTokenGetter` stub in `custom-fetch.ts`
- Firebase Firestore → real-time device readings (alongside or replacing PostgreSQL for time-series)
- Firebase Cloud Messaging → push notifications

### IoT Ingestion (v0.5.0)
- New Express route: `POST /api/readings` — accepts ESP32 sensor payloads
- Validates payload with Zod, writes to PostgreSQL/Firestore

### PWA (v0.7.0)
- Web App Manifest in the frontend build
- Service Worker with Workbox for offline caching

---

## TypeScript Configuration

| File | Role |
|------|------|
| `tsconfig.base.json` | Shared strict defaults (ESNext, `bundler` module resolution) |
| `tsconfig.json` (root) | Solution file for composite `lib/*` packages only |
| `tsconfig.json` (each package) | Extends base; `artifacts/*` use `noEmit: true` |

**Do not** add `artifacts/*` to the root `tsconfig.json` references — that file is for buildable libs only.
