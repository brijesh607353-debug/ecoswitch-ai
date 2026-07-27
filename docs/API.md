# API Reference — EcoSwitch AI

> **Reflects implemented endpoints only.** Planned endpoints are clearly labelled.

The full contract lives in [`lib/api-spec/openapi.yaml`](../lib/api-spec/openapi.yaml) — that file is the single source of truth. This document provides human-readable usage examples.

---

## Base URL

| Environment | Base URL |
|-------------|----------|
| Local dev | `http://localhost:5000/api` |
| Production | `https://your-domain.com/api` |

---

## Authentication

> **Status: Planned.** Currently all endpoints are unauthenticated.

When implemented, the API will use **Bearer token authentication** via Firebase Auth ID tokens:

```
Authorization: Bearer <firebase-id-token>
```

The client-side hook `setAuthTokenGetter` in `lib/api-client-react/src/custom-fetch.ts` is already wired to attach tokens to every request — it just needs a Firebase Auth token source.

---

## Implemented Endpoints

### `GET /api/healthz`

Returns the health status of the API server. No authentication required.

**Request**
```bash
curl http://localhost:5000/api/healthz
```

**Response `200 OK`**
```json
{
  "status": "ok"
}
```

**Response schema** (Zod, auto-generated):
```typescript
// lib/api-zod/src/generated/types/healthStatus.ts
export const healthStatusSchema = z.object({
  status: z.string(),
});
```

**React Query hook** (auto-generated):
```typescript
import { useHealthCheck } from "@workspace/api-client-react";

function StatusBadge() {
  const { data, isLoading } = useHealthCheck();
  return <span>{isLoading ? "checking…" : data?.status}</span>;
}
```

---

## Planned Endpoints

These endpoints are in the roadmap and will be added to `openapi.yaml` as they are implemented.

### Authentication

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/auth/register` | Register a new user |
| `POST` | `/api/auth/login` | Sign in and receive a session token |
| `POST` | `/api/auth/logout` | Sign out |
| `GET` | `/api/auth/me` | Get the authenticated user profile |

### Devices (IoT)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/devices` | List all devices for the authenticated user |
| `POST` | `/api/devices` | Register a new ESP32 device |
| `GET` | `/api/devices/:id` | Get a single device |
| `PATCH` | `/api/devices/:id` | Update device settings |
| `DELETE` | `/api/devices/:id` | Remove a device |
| `POST` | `/api/readings` | Ingest a sensor reading (called by ESP32) |

### Energy Data

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/energy` | Paginated energy readings |
| `GET` | `/api/energy/summary` | Aggregated usage for a time period |
| `GET` | `/api/energy/comparison` | Compare current vs previous period |

### Analytics & Reports

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/analytics/trends` | Usage trend data for charts |
| `GET` | `/api/analytics/anomalies` | Detected usage anomalies |
| `POST` | `/api/reports/pdf` | Generate and download a PDF report |

### Notifications

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/notifications` | List user notifications |
| `PATCH` | `/api/notifications/:id` | Mark a notification as read |

---

## Codegen Workflow

After editing `lib/api-spec/openapi.yaml`:

```bash
pnpm --filter @workspace/api-spec run codegen
```

This regenerates:
- `lib/api-client-react/src/generated/api.ts` — React Query hooks
- `lib/api-client-react/src/generated/api.schemas.ts` — TypeScript interfaces
- `lib/api-zod/src/generated/api.ts` — Zod schemas
- `lib/api-zod/src/generated/types/` — TypeScript types

**Do not edit generated files manually** — they are overwritten on every codegen run.

---

## Error Handling

The API uses standard HTTP status codes. Error response format (planned):

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request body",
    "details": [
      { "field": "name", "message": "Required" }
    ]
  }
}
```
