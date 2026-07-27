# Testing Guide — EcoSwitch AI

> **Current state:** No automated tests exist yet. The TypeScript typecheck pipeline is the only automated quality gate. This document describes the intended testing strategy.

---

## Current Quality Gates

| Check | Command | Status |
|-------|---------|--------|
| TypeScript (all packages) | `pnpm run typecheck` | ✅ Passing |
| Prettier formatting | `pnpm prettier --check .` | ✅ Configured |
| Unit tests | — | 🔨 Not yet written |
| Integration tests | — | 🔨 Not yet written |
| End-to-end tests | — | 🔨 Not yet written |

---

## Planned Testing Strategy

### Unit Tests (Planned: Vitest)

**Target:** Pure functions — Zod schema validation, utility functions, data transformations.

```bash
# Will be added when tests are introduced
pnpm --filter @workspace/api-server run test
pnpm --filter @workspace/api-server run test:watch
```

**Planned test locations:**
- `artifacts/api-server/src/**/*.test.ts`
- `lib/db/src/**/*.test.ts`

**Example (planned):**
```typescript
import { describe, it, expect } from "vitest";
import { healthStatusSchema } from "@workspace/api-zod";

describe("healthStatusSchema", () => {
  it("accepts a valid status object", () => {
    expect(healthStatusSchema.parse({ status: "ok" })).toEqual({ status: "ok" });
  });

  it("rejects missing status", () => {
    expect(() => healthStatusSchema.parse({})).toThrow();
  });
});
```

### Integration Tests (Planned: Vitest + Supertest)

**Target:** Express route handlers end-to-end, including middleware and DB interactions.

```typescript
// Planned: artifacts/api-server/src/routes/health.test.ts
import request from "supertest";
import app from "../app";

describe("GET /api/healthz", () => {
  it("returns 200 with status ok", async () => {
    const res = await request(app).get("/api/healthz");
    expect(res.status).toBe(200);
    expect(res.body).toEqual({ status: "ok" });
  });
});
```

### End-to-End Tests (Planned: Playwright)

**Target:** Critical user flows in the browser — login, dashboard view, device management.

---

## Setting Up Tests (When Ready)

```bash
# Add Vitest to the API server package
pnpm --filter @workspace/api-server add -D vitest @vitest/coverage-v8

# Add Supertest for HTTP integration tests
pnpm --filter @workspace/api-server add -D supertest @types/supertest

# Add Playwright for E2E (root level)
pnpm add -D -w @playwright/test
npx playwright install
```

Add to `artifacts/api-server/package.json`:
```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:coverage": "vitest run --coverage"
  }
}
```

---

## CI Integration

The GitHub Actions workflow (`.github/workflows/ci.yml`) currently runs `typecheck` and `prettier`. A `test` job will be added when unit tests exist:

```yaml
# Will be added to .github/workflows/ci.yml
test:
  name: Tests
  runs-on: ubuntu-latest
  services:
    postgres:
      image: postgres:15
      env:
        POSTGRES_PASSWORD: test
        POSTGRES_DB: ecoswitch_test
  steps:
    - uses: actions/checkout@v4
    - uses: pnpm/action-setup@v4
    - run: pnpm install --frozen-lockfile
    - run: pnpm --filter @workspace/api-server run test
      env:
        DATABASE_URL: postgresql://postgres:test@localhost:5432/ecoswitch_test
```
