# EcoSwitch AI — Documentation

Welcome to the EcoSwitch AI documentation hub. This folder contains in-depth guides that go beyond the top-level README.

---

## Contents

| Document | Description |
|----------|-------------|
| _(coming soon)_ `architecture.md` | System architecture overview and data flow diagrams |
| _(coming soon)_ `api.md` | Detailed API reference and example requests |
| _(coming soon)_ `database-schema.md` | Database schema, relationships, and migration guide |
| _(coming soon)_ `deployment.md` | Production deployment guide (Docker, cloud providers) |
| _(coming soon)_ `testing.md` | Testing strategy, running tests, and writing new tests |
| _(coming soon)_ `adr/` | Architecture Decision Records |

---

## Quick Links

- [Main README](../README.md)
- [OpenAPI Spec](../lib/api-spec/openapi.yaml) — single source of truth for all API contracts
- [Contributing Guide](../CONTRIBUTING.md)
- [Security Policy](../SECURITY.md)
- [Changelog](../CHANGELOG.md)

---

## Adding Documentation

To add a new doc:

1. Create a Markdown file in this `docs/` folder (e.g. `docs/deployment.md`)
2. Add it to the table above
3. Link to it from the relevant section of the main README

Keep documentation close to the code it describes — if a doc is specific to one package (e.g. the API server), consider adding it inside that package's folder as well.
