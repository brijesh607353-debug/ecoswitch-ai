# Development Guide — EcoSwitch AI

Everything you need to work on the codebase day-to-day.

---

## Prerequisites

| Tool | Minimum | Recommended | Install |
|------|---------|-------------|---------|
| Node.js | 20 | 24 | [nodejs.org](https://nodejs.org/) |
| pnpm | 9 | latest | `npm install -g pnpm` |
| PostgreSQL | 15 | 15 | [postgresql.org](https://www.postgresql.org/download/) |
| Git | 2.30 | latest | [git-scm.com](https://git-scm.com/) |

---

## Initial Setup

```bash
# 1. Clone the repo
git clone https://github.com/brijesh607353-debug/ecoswitch-ai.git
cd ecoswitch-ai

# 2. Install all workspace dependencies
pnpm install

# 3. Set up environment variables
cp .env.example .env.local
# Edit .env.local — at minimum set DATABASE_URL and SESSION_SECRET

# 4. Generate a session secret
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"

# 5. Push the database schema (currently empty — creates the connection)
pnpm --filter @workspace/db run push

# 6. Start the API server
pnpm --filter @workspace/api-server run dev

# 7. Verify
curl http://localhost:5000/api/healthz
# → {"status":"ok"}
```

---

## Key Commands

### API Server

```bash
# Start in development mode (auto-restart on change)
pnpm --filter @workspace/api-server run dev

# Build a CJS production bundle
pnpm --filter @workspace/api-server run build

# TypeScript check only
pnpm --filter @workspace/api-server run typecheck
```

### Code Generation

```bash
# After editing lib/api-spec/openapi.yaml:
pnpm --filter @workspace/api-spec run codegen
# Generates:
#   lib/api-client-react/src/generated/api.ts         — React Query hooks
#   lib/api-client-react/src/generated/api.schemas.ts — TypeScript interfaces
#   lib/api-zod/src/generated/api.ts                  — Zod schemas
#   lib/api-zod/src/generated/types/                  — TypeScript types
```

> ⚠️ Never edit generated files manually — they are overwritten on every codegen run.

### Database

```bash
# Push schema to local database (development only — skips migration files)
pnpm --filter @workspace/db run push

# Generate migration files (use for staging/production)
pnpm --filter @workspace/db run generate

# Apply pending migrations
pnpm --filter @workspace/db run migrate
```

### TypeScript

```bash
# Full check across all packages (run before every commit)
pnpm run typecheck

# Rebuild lib declarations only
pnpm run typecheck:libs
```

### Formatting

```bash
# Format all files
pnpm prettier --write .

# Check formatting (this is what CI runs)
pnpm prettier --check .
```

### Full Build

```bash
# typecheck + esbuild bundle for all packages
pnpm run build
```

---

## Contract-First API Development

EcoSwitch AI follows a strict contract-first workflow. Every API endpoint must be defined in the OpenAPI spec **before** its handler is written.

```
1. Edit lib/api-spec/openapi.yaml
   ↓
2. pnpm --filter @workspace/api-spec run codegen
   ↓
3. Zod schemas appear in lib/api-zod/src/generated/
   React Query hooks appear in lib/api-client-react/src/generated/
   ↓
4. Implement the Express route handler
   (import + use generated Zod schema for validation)
   ↓
5. Use generated hook in the frontend
   (import from @workspace/api-client-react)
```

### Adding a New Endpoint — Checklist

- [ ] Add path + operation to `lib/api-spec/openapi.yaml`
- [ ] Run codegen — verify generated hook and schema names
- [ ] Create or update route file in `artifacts/api-server/src/routes/`
- [ ] Register route in `artifacts/api-server/src/routes/index.ts`
- [ ] Validate request body with generated Zod schema
- [ ] Use `req.log` for logging (not `console.log`)
- [ ] Run `pnpm run typecheck` — must pass

---

## Adding a Database Table

```typescript
// lib/db/src/schema/devices.ts
import { pgTable, text, serial, timestamp } from "drizzle-orm/pg-core";
import { createInsertSchema } from "drizzle-zod";
import { z } from "zod/v4";

export const devicesTable = pgTable("devices", {
  id:        serial("id").primaryKey(),
  name:      text("name").notNull(),
  userId:    text("user_id").notNull(),
  location:  text("location"),
  createdAt: timestamp("created_at").defaultNow().notNull(),
});

export const insertDeviceSchema = createInsertSchema(devicesTable).omit({ id: true, createdAt: true });
export type InsertDevice = z.infer<typeof insertDeviceSchema>;
export type Device = typeof devicesTable.$inferSelect;
```

Then export from the barrel:

```typescript
// lib/db/src/schema/index.ts
export * from "./devices";
```

Then push:

```bash
pnpm --filter @workspace/db run push
```

---

## Logging — Server Side

**Never use `console.log` in server code.**

| Context | Use |
|---------|-----|
| Inside a route handler | `req.log.info({ data }, "message")` |
| Outside a request | `import { logger } from "./lib/logger"; logger.info(...)` |
| Error with stack | `req.log.error({ err }, "message")` |

Pino serialises logs as JSON in production and pretty-prints in development (`NODE_ENV !== "production"`).

---

## Monorepo Rules

1. **`artifacts/*` packages must never import from each other** — shared code goes in `lib/*`
2. **`lib/*` packages are composite** (they emit declarations via `tsc --build`)
3. **`artifacts/*` packages are leaf packages** (`noEmit: true` — do not add them to root `tsconfig.json` references)
4. **Generated packages** (`lib/api-client-react`, `lib/api-zod`) must never be edited manually
5. **All new shared dependencies** — check `pnpm-workspace.yaml` catalog first; use `catalog:` if already pinned

---

## Environment Variables

See [`.env.example`](../.env.example) for the complete list.

Minimum for local development:

```env
DATABASE_URL=postgresql://postgres:password@localhost:5432/ecoswitch
SESSION_SECRET=<32-byte hex string>
NODE_ENV=development
PORT=5000
```

---

## Troubleshooting

See the [Troubleshooting section in the README](../README.md#troubleshooting) for common issues and fixes.

---

## Pull Request Checklist

Before opening a PR:

```bash
pnpm run typecheck      # must pass
pnpm prettier --check . # must pass
# If you edited openapi.yaml:
pnpm --filter @workspace/api-spec run codegen
git add lib/api-client-react lib/api-zod
```

Fill in the PR template. Link related issues with `Closes #N`.
