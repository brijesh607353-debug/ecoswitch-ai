# Architecture — EcoSwitch AI

> Reflects the actual codebase as of v0.1.0. Planned components are clearly labelled.

---

## System Architecture

```mermaid
flowchart TB
    subgraph iot["🔌 IoT Layer · Planned"]
        SEN["Sensors\nSCT-013 · ZMPT101B"]
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
        FB[("Firebase Firestore\n📌 Planned")]
        ORM --> PG
    end

    subgraph frontend["🖥️ Frontend · In Development"]
        REACT["React 19 + Vite 6"]
        RQ["TanStack React Query v5"]
        UI["shadcn/ui · 50+ components"]
        CHARTS["Recharts · 📌 Planned"]
        UI --> REACT
        CHARTS --> REACT
        RQ --> REACT
    end

    ESP -->|"POST /api/readings · 📌 Planned"| API
    REACT -->|"Generated React Query hooks"| API
    ZOD --> ORM
    API -.->|"📌 Planned"| FB
```

---

## Monorepo Layout

```mermaid
flowchart LR
    subgraph libs["lib/ — Shared Libraries"]
        SPEC["api-spec\nopenapi.yaml"]
        CODEGEN["Orval codegen"]
        HOOKS["api-client-react\nGenerated hooks"]
        SCHEMAS["api-zod\nGenerated schemas"]
        DB["db\nDrizzle ORM"]
        SPEC --> CODEGEN
        CODEGEN --> HOOKS
        CODEGEN --> SCHEMAS
    end

    subgraph artifacts["artifacts/ — Applications"]
        SERVER["api-server\nExpress 5"]
        SANDBOX["mockup-sandbox\nReact + Vite"]
    end

    SCHEMAS --> SERVER
    HOOKS --> SANDBOX
    DB --> SERVER
```

| Package | Path | Role |
|---------|------|------|
| `@workspace/api-server` | `artifacts/api-server/` | Express 5 REST API — the only deployable backend |
| `@workspace/mockup-sandbox` | `artifacts/mockup-sandbox/` | React + Vite component sandbox (dev only) |
| `@workspace/api-spec` | `lib/api-spec/` | OpenAPI 3.1 spec + Orval codegen config |
| `@workspace/api-client-react` | `lib/api-client-react/` | **Generated** React Query hooks (do not edit) |
| `@workspace/api-zod` | `lib/api-zod/` | **Generated** Zod schemas + TypeScript types (do not edit) |
| `@workspace/db` | `lib/db/` | Drizzle ORM schema, migrations, DB client |
| `@workspace/scripts` | `scripts/` | Utility scripts |

**Rules:**
- `artifacts/*` packages must never import from each other — shared logic lives in `lib/*`
- Generated packages (`api-client-react`, `api-zod`) must never be edited manually — regenerate with codegen
- `lib/*` packages are composite TypeScript (emit declarations); `artifacts/*` are leaf packages (no emit)

---

## Contract-First API Design

```mermaid
flowchart LR
    SPEC["lib/api-spec/openapi.yaml\n← Edit here"]
    GEN["Orval Codegen\npnpm --filter api-spec run codegen"]
    HOOKS["lib/api-client-react\nReact Query hooks"]
    SCHEMAS["lib/api-zod\nZod schemas + TS types"]
    SERVER["artifacts/api-server\nExpress route handlers"]
    CLIENT["artifacts/mockup-sandbox\nReact components"]

    SPEC --> GEN
    GEN --> HOOKS
    GEN --> SCHEMAS
    SCHEMAS -->|"validate inputs"| SERVER
    HOOKS -->|"fetch data"| CLIENT
```

**Workflow:**
1. Design the endpoint in `openapi.yaml` first
2. Run codegen — hooks and schemas appear automatically
3. Implement the Express route handler using the generated Zod schema for validation
4. The frontend consumes the generated React Query hook — no manual type copying

---

## API Request Lifecycle

```mermaid
sequenceDiagram
    participant C as React Client
    participant Q as React Query
    participant A as Express API
    participant Z as Zod Schema
    participant D as Drizzle / PostgreSQL

    C->>Q: useHealthCheck()
    Q->>A: GET /api/healthz
    A->>Z: validate response shape
    Z-->>A: HealthStatus { status }
    A-->>Q: 200 { "status": "ok" }
    Q-->>C: data.status

    Note over C,D: Future authenticated flow (📌 Planned)
    C->>Q: useCreateReading(payload)
    Q->>A: POST /api/readings + Bearer token
    A->>Z: validate sensor payload
    Z->>D: insert reading
    D-->>A: confirmed
    A-->>Q: 201 Created
    Q-->>C: invalidate queries
```

---

## API Server Internal Structure

```mermaid
flowchart TD
    REQ["Incoming HTTP Request"]
    PINO["pino-http\nRequest logging"]
    CORS["cors\nCross-origin"]
    JSON["express.json()\nBody parsing"]
    ROUTER["Router /api"]
    HEALTH["GET /healthz\n✅ Implemented"]
    AUTH["Auth middleware\n📌 Planned v0.3.0"]
    DEVICES["/devices CRUD\n📌 Planned v0.2.0"]
    READINGS["POST /readings\n📌 Planned v0.5.0"]

    REQ --> PINO --> CORS --> JSON --> ROUTER
    ROUTER --> HEALTH
    ROUTER --> AUTH
    AUTH --> DEVICES
    AUTH --> READINGS
```

**Files:**

| File | Purpose |
|------|---------|
| `index.ts` | Process entry point — binds to `process.env.PORT` |
| `app.ts` | Express factory — registers middleware and router |
| `routes/index.ts` | Route aggregator — mounts all sub-routers |
| `routes/health.ts` | `GET /api/healthz` handler |
| `middlewares/` | Empty — ready for auth, rate-limiting |
| `lib/logger.ts` | Pino singleton — use `req.log` in handlers |

---

## Data Layer

**Path:** `lib/db/`

```mermaid
flowchart LR
    SCHEMA["lib/db/src/schema/\nDrizzle table definitions"]
    DZ["drizzle-zod\nSchema → Zod types"]
    DRIZZLE["Drizzle ORM\nQuery builder"]
    PG[("PostgreSQL\n✅ Connected")]
    KIT["Drizzle Kit\nMigrations"]

    SCHEMA --> DZ
    SCHEMA --> DRIZZLE
    DRIZZLE --> PG
    SCHEMA --> KIT
    KIT --> PG
```

- **ORM:** Drizzle ORM with `drizzle-orm/node-postgres`
- **Migrations:** Drizzle Kit (`push` for dev, `generate`+`migrate` for production)
- **Schema validation:** `drizzle-zod` generates Zod schemas from Drizzle table definitions
- **Current state:** Connection configured; no tables defined yet (v0.2.0)

**Adding a new table:**
```typescript
// lib/db/src/schema/devices.ts
import { pgTable, text, serial, timestamp } from "drizzle-orm/pg-core";
import { createInsertSchema } from "drizzle-zod";
import { z } from "zod/v4";

export const devicesTable = pgTable("devices", {
  id:        serial("id").primaryKey(),
  name:      text("name").notNull(),
  userId:    text("user_id").notNull(),
  createdAt: timestamp("created_at").defaultNow().notNull(),
});

export const insertDeviceSchema = createInsertSchema(devicesTable).omit({ id: true });
export type InsertDevice = z.infer<typeof insertDeviceSchema>;
export type Device = typeof devicesTable.$inferSelect;
```

---

## Planned Additions

### Firebase (v0.3.0)
- Firebase Auth replaces the `setAuthTokenGetter` stub in `custom-fetch.ts`
- Firebase Firestore for real-time readings (alongside PostgreSQL)
- Firebase Cloud Messaging for push notifications

### IoT Ingestion (v0.5.0)
- `POST /api/readings` — accepts and validates ESP32 sensor payloads
- Writes to PostgreSQL time-series table via Drizzle

### PWA (v0.7.0)
- Web App Manifest + Service Worker (Workbox)
- Offline read-only dashboard with background sync

---

## TypeScript Configuration

| File | Role |
|------|------|
| `tsconfig.base.json` | Shared strict defaults (ESNext, `bundler` module resolution) |
| `tsconfig.json` (root) | Solution file for composite `lib/*` packages only |
| Each package `tsconfig.json` | Extends base; `artifacts/*` use `noEmit: true` |

**Do not** add `artifacts/*` to the root `tsconfig.json` references — that file is for buildable libs only.
