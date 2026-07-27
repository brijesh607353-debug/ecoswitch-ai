# Roadmap — EcoSwitch AI

This is the authoritative milestone plan. Status is updated on every release.

---

## Legend

| Symbol | Meaning |
|--------|---------|
| ✅ | Complete — merged to `main` |
| 🚧 | In Progress — actively being worked |
| 📌 | Planned — scoped, not yet started |
| 💡 | Idea — under consideration, not scoped |

---

## v0.1.0 — Foundation ✅ Released 2026-07-27

The engineering backbone: monorepo, API server, database scaffolding, codegen pipeline, and full GitHub repository quality.

| Item | Status |
|------|--------|
| pnpm monorepo workspace | ✅ |
| Express 5 REST API server | ✅ |
| OpenAPI 3.1 spec (`lib/api-spec/openapi.yaml`) | ✅ |
| Orval codegen → React Query hooks + Zod schemas | ✅ |
| PostgreSQL + Drizzle ORM (connection layer) | ✅ |
| `GET /api/healthz` health check endpoint | ✅ |
| Pino structured logging | ✅ |
| 50+ shadcn/ui components in Vite sandbox | ✅ |
| Client-side auth token hook (stub) | ✅ |
| GitHub Actions CI (typecheck + prettier) | ✅ |
| Dependabot (weekly dependency updates) | ✅ |
| Premium README, docs, issue templates | ✅ |

---

## v0.2.0 — Data Layer 🚧 In Progress

Define the database schema and expose the first real CRUD endpoints.

| Item | Status |
|------|--------|
| `devices` table (Drizzle schema) | 🚧 |
| `users` table | 🚧 |
| `energy_readings` table | 🚧 |
| `GET /api/devices` — list devices | 📌 |
| `POST /api/devices` — register device | 📌 |
| `GET /api/devices/:id` | 📌 |
| `PATCH /api/devices/:id` | 📌 |
| `DELETE /api/devices/:id` | 📌 |
| Drizzle migrations pipeline | 📌 |
| Unit tests for Zod schemas | 📌 |
| Integration tests for routes (Vitest + Supertest) | 📌 |
| Seed script for local development | 📌 |

---

## v0.3.0 — Authentication 📌 Planned

Secure every endpoint. Integrate Firebase Auth.

| Item | Status |
|------|--------|
| Firebase project setup | 📌 |
| Firebase Auth client-side integration | 📌 |
| Firebase Admin SDK server-side verification | 📌 |
| `POST /api/auth/register` | 📌 |
| `POST /api/auth/login` | 📌 |
| `GET /api/auth/me` | 📌 |
| Auth middleware (`requireAuth`) | 📌 |
| Protect all device and energy endpoints | 📌 |
| Rate limiting (`express-rate-limit`) | 📌 |

---

## v0.4.0 — Dashboard 📌 Planned

The main user-facing product. Real-time energy data, Recharts visualisations.

| Item | Status |
|------|--------|
| Production React + Vite frontend app | 📌 |
| Energy consumption chart (24h, 7d, 30d) | 📌 |
| Device-level breakdown | 📌 |
| Cost vs CO₂ toggle | 📌 |
| Historical trend comparisons | 📌 |
| Anomaly alert banners | 📌 |
| Dark/light mode (next-themes) | 📌 |
| Framer Motion page transitions | 📌 |

---

## v0.5.0 — IoT Integration 📌 Planned

Connect real hardware. Requires ESP32 and sensors.

| Item | Status |
|------|--------|
| `POST /api/readings` — sensor ingestion endpoint | 📌 |
| Device API key authentication | 📌 |
| Sensor payload Zod validation | 📌 |
| ESP32 Arduino firmware sketch | 📌 |
| Wi-Fi config + HTTPS client | 📌 |
| ADC reading (SCT-013 + ZMPT101B) | 📌 |
| Mock sensor simulator script | 📌 |
| Hardware setup guide (`docs/IOT.md`) | 📌 |

> ⚠️ This milestone requires physical hardware (ESP32 + sensors).

---

## v0.6.0 — Analytics & Reports 📌 Planned

Aggregate insights and exportable reports.

| Item | Status |
|------|--------|
| `GET /api/analytics/trends` | 📌 |
| `GET /api/analytics/anomalies` | 📌 |
| `GET /api/energy/summary` | 📌 |
| `GET /api/energy/comparison` | 📌 |
| `POST /api/reports/pdf` — PDF generation | 📌 |
| Firebase Cloud Messaging push notifications | 📌 |
| `GET /api/notifications` | 📌 |
| Usage spike alert system | 📌 |
| Monthly usage email report | 📌 |

---

## v0.7.0 — PWA & Offline 📌 Planned

Install the app, use it offline.

| Item | Status |
|------|--------|
| Web App Manifest | 📌 |
| Service Worker (Workbox) | 📌 |
| Offline dashboard (last cached readings) | 📌 |
| Background sync (queue writes when offline) | 📌 |
| "Add to Home Screen" install prompt | 📌 |
| App icon set (192px, 512px) | 📌 |
| Lighthouse PWA score ≥ 90 | 📌 |

---

## v1.0.0 — Production Release 📌 Planned

Performance-tuned, fully tested, production-hardened.

| Item | Status |
|------|--------|
| Full E2E test coverage (Playwright) | 📌 |
| Performance benchmarks (API < 20ms p50) | 📌 |
| Lighthouse Performance ≥ 90 | 📌 |
| HTTPS + TLS enforced | 📌 |
| Docker production image | 📌 |
| CI/CD deployment pipeline | 📌 |
| Security audit | 📌 |
| Full documentation audit | 📌 |

---

## Future Ideas 💡

Not scoped — ideas for consideration after v1.0.0.

| Idea | Notes |
|------|-------|
| AI energy recommendation engine | OpenAI API or local model |
| Energy supplier comparison API | Third-party integration |
| Multi-household / team support | RBAC + org model |
| MQTT support for high-frequency IoT | Alternative to HTTP |
| Mobile app (React Native / Expo) | Same API, native shell |
| Energy market pricing API | Real-time tariff data |
| Blockchain carbon credit tracking | Long-term research |

---

## Contributing to the Roadmap

If you want to tackle a `📌 Planned` item:
1. Check if an issue exists — if not, open one
2. Comment that you're working on it (prevents duplication)
3. Follow the [contributing guide](../CONTRIBUTING.md)

Feature ideas go in [GitHub Discussions](https://github.com/brijesh607353-debug/ecoswitch-ai/discussions).
