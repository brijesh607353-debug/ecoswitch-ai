# Performance — EcoSwitch AI

> **Current state:** No performance benchmarks have been run. This document describes built-in performance characteristics and the planned measurement strategy.

---

## Current Performance Characteristics

### API Server

| Aspect | Implementation | Impact |
|--------|---------------|--------|
| Logging | Pino (fastest Node.js logger) | Minimal logging overhead |
| JSON parsing | Express built-in | Standard |
| Request serialisation | Custom Pino serialiser (strips query params from logs) | Reduces log volume |
| Bundle | esbuild CJS bundle | Fast cold start |

### TypeScript Pipeline

| Operation | Typical time |
|-----------|-------------|
| `pnpm run typecheck` (full) | ~3-5 seconds |
| Orval codegen | < 2 seconds |
| esbuild bundle | < 1 second |

---

## Planned Performance Work

### API Performance

- **Response compression** — add `compression` middleware for large JSON responses
- **Database connection pooling** — configure `pg` pool size based on load
- **Query optimisation** — add indexes when Drizzle schema is defined
- **Rate limiting** — `express-rate-limit` on all endpoints

### Frontend Performance

> The production frontend is not yet built. When it is:

- **Code splitting** — Vite's built-in automatic chunking
- **Tree shaking** — Radix UI components are individually imported
- **Image optimisation** — WebP format, lazy loading
- **Bundle analysis** — `vite-bundle-analyzer` to track size regressions

### Planned Measurements

| Metric | Tool | Target |
|--------|------|--------|
| API latency (p50, p99) | `autocannon` or `k6` | p50 < 20ms, p99 < 100ms |
| Frontend bundle size | Vite build report | < 200KB initial JS |
| Lighthouse Performance | Lighthouse CLI | ≥ 90 |
| Lighthouse PWA | Lighthouse CLI | ≥ 90 (after PWA implementation) |
| Time to Interactive | Lighthouse | < 3s on 4G |

> ⚠️ **No Lighthouse scores are claimed** — these are targets for future measurement.

---

## Logging Strategy (Implemented)

Pino is configured to reduce noise in production:

- Query string parameters are stripped from request URL logs (prevents leaking filter values)
- Only method, path (no query), and status code are logged per request
- `pino-pretty` is enabled only in development (`NODE_ENV !== 'production'`)

```typescript
// artifacts/api-server/src/app.ts
serializers: {
  req(req) {
    return {
      id: req.id,
      method: req.method,
      url: req.url?.split("?")[0],  // ← strips query string
    };
  },
}
```
