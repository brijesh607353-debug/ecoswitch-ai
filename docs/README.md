# EcoSwitch AI — Documentation

Technical documentation for the EcoSwitch AI project. Every document reflects the actual codebase — status labels make it clear what exists and what is planned.

---

## Index

| Document | Status | Description |
|----------|:------:|-------------|
| [ARCHITECTURE.md](./ARCHITECTURE.md) | ✅ Current | Monorepo layout, Mermaid system diagrams, contract-first API pattern |
| [API.md](./API.md) | ✅ Current | Endpoint reference, codegen workflow, planned routes |
| [DEVELOPMENT.md](./DEVELOPMENT.md) | ✅ Current | Day-to-day development commands, workflow, monorepo rules |
| [DEPLOYMENT.md](./DEPLOYMENT.md) | ✅ Current | Docker, Railway, VPS deployment for the Express API |
| [ROADMAP.md](./ROADMAP.md) | ✅ Current | Milestone plan v0.1.0 → v1.0.0 with status per item |
| [FIREBASE.md](./FIREBASE.md) | 📌 Planned | Firebase setup guide (not yet integrated) |
| [IOT.md](./IOT.md) | 📌 Planned | ESP32 hardware integration architecture |
| [PWA.md](./PWA.md) | 📌 Planned | Progressive Web App implementation plan |
| [TESTING.md](./TESTING.md) | 📌 Planned | Testing strategy (no tests exist yet) |
| [PERFORMANCE.md](./PERFORMANCE.md) | ✅ Current | Built-in Pino choices + planned benchmarks |
| [KNOWN_LIMITATIONS.md](./KNOWN_LIMITATIONS.md) | ✅ Current | Honest record of what is not yet implemented |
| [GITHUB_LABELS.md](./GITHUB_LABELS.md) | ✅ Current | Issue and PR label taxonomy |

---

## Quick Links

- [Main README](../README.md) — project overview, architecture diagrams, getting started
- [OpenAPI Spec](../lib/api-spec/openapi.yaml) — single source of truth for all API contracts
- [Contributing Guide](../CONTRIBUTING.md)
- [Security Policy](../SECURITY.md)
- [Changelog](../CHANGELOG.md)
- [Roadmap](./ROADMAP.md)

---

## Documentation Standards

1. **Only document what exists** — use `✅ Implemented`, `🚧 In Progress`, `📌 Planned` labels
2. **Keep docs close to code** — package-specific docs can live inside that package folder too
3. **Update CHANGELOG.md** on every meaningful change
4. **Update this index** when adding a new doc
5. **Never claim** hardware tested, Lighthouse scores, or live deployments unless verifiable
