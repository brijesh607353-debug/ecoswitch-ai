# EcoSwitch AI — Documentation

This folder contains in-depth technical documentation for EcoSwitch AI.

---

## Index

| Document | Status | Description |
|----------|--------|-------------|
| [ARCHITECTURE.md](./ARCHITECTURE.md) | ✅ Current | Monorepo layout, system design, contract-first API pattern |
| [API.md](./API.md) | ✅ Current | Endpoint reference, codegen workflow, planned routes |
| [FIREBASE.md](./FIREBASE.md) | 🔨 Planned | Firebase setup guide (integration not yet implemented) |
| [IOT.md](./IOT.md) | 🔨 Planned | ESP32 sensor integration architecture |
| [PWA.md](./PWA.md) | 🔨 Planned | Progressive Web App implementation plan |
| [DEPLOYMENT.md](./DEPLOYMENT.md) | ✅ Current | API server and database deployment (Docker, Railway, VPS) |
| [TESTING.md](./TESTING.md) | 🔨 Planned | Testing strategy and setup (no tests exist yet) |
| [PERFORMANCE.md](./PERFORMANCE.md) | ✅ Current | Built-in performance choices + planned benchmarks |
| [KNOWN_LIMITATIONS.md](./KNOWN_LIMITATIONS.md) | ✅ Current | Honest record of what is not yet implemented |

---

## Quick Links

- [Main README](../README.md) — project overview and getting started
- [OpenAPI Spec](../lib/api-spec/openapi.yaml) — single source of truth for API contracts
- [Contributing Guide](../CONTRIBUTING.md)
- [Security Policy](../SECURITY.md)
- [Changelog](../CHANGELOG.md)

---

## Adding Documentation

1. Create a Markdown file in this `docs/` folder
2. Add it to the index table above with its status
3. Link to it from the relevant section of the main README
4. Only document what **actually exists** — use status labels (`✅ Current`, `🔨 Planned`) to be explicit
