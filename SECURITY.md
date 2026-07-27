# Security Policy — EcoSwitch AI

## Reporting a Vulnerability

**Please do not report security vulnerabilities through public GitHub issues.**

If you discover a security vulnerability, report it privately so we can fix it before it is exploited.

### How to Report

**Option 1 — GitHub Private Vulnerability Reporting (preferred)**
Use [GitHub's private security advisory feature](https://github.com/brijesh607353-debug/ecoswitch-ai/security/advisories/new).

**Option 2 — Direct message**
Reach the maintainer directly via [GitHub profile](https://github.com/brijesh607353-debug).

Please include:

| Field | Description |
|-------|-------------|
| **Type** | e.g. SQL injection, XSS, auth bypass, secret exposure |
| **Location** | File path, endpoint, or component |
| **Steps to reproduce** | Minimal, reliable reproduction steps |
| **Proof of concept** | Code snippet or curl command (if safe to share) |
| **Impact** | What an attacker could do if exploited |
| **Suggested fix** | Optional — appreciated but not required |

### Response Timeline

| Milestone | Timeline |
|-----------|---------|
| Acknowledgement | Within 48 hours |
| Initial triage | Within 7 days |
| Fix released | Within 30 days (for confirmed vulnerabilities) |
| Public disclosure | Coordinated with reporter after fix |

We follow a **90-day coordinated disclosure** policy.

---

## Supported Versions

| Version | Supported |
|---------|-----------|
| Latest (`main`) | ✅ |
| Older releases | ❌ — upgrade to latest |

---

## Current Security Posture (v0.1.0)

This section is updated with every release.

| Area | Status | Notes |
|------|--------|-------|
| Authentication | ⚠️ None | All endpoints unauthenticated — do not expose to the public internet |
| HTTPS / TLS | ⚠️ Not configured | Use a reverse proxy (nginx, Caddy) or platform-level TLS in production |
| Input validation | ✅ Zod | All API inputs validated via Zod schemas |
| SQL injection | ✅ Protected | Drizzle ORM uses parameterised queries |
| Secret management | ✅ Env vars | No credentials in code; `.env*` files are git-ignored |
| Dependency audit | 🔨 Not automated | Dependabot PRs will flag vulnerable dependencies |
| Rate limiting | 🔨 Planned | To be added in v0.2.0 |
| CORS | ✅ Enabled | `cors` middleware applied; restrict `origin` in production |

---

## Security Best Practices for Contributors

### Secrets & Credentials

- **Never commit** `.env`, `.env.local`, or files containing real credentials
- `.env.example` uses only empty values — it is safe to commit
- Rotate any credential immediately if you suspect accidental exposure
- Firebase Admin SDK keys (`service-account.json`) are git-ignored — never commit them

### Dependencies

- Run `pnpm audit` before submitting a PR that adds or updates dependencies
- Avoid packages with known CVEs or that are unmaintained
- Use `catalog:` pins in `pnpm-workspace.yaml` for shared deps

### Input Validation

- All API inputs must be validated with Zod schemas (generated from OpenAPI where possible)
- Never concatenate user input into SQL strings — Drizzle's query builder prevents this
- Strip sensitive fields from Pino log output (query params are already stripped)

### Authentication (When Implemented)

- Verify Firebase ID tokens server-side using the Admin SDK on every protected request
- Never trust client-supplied user IDs — always derive identity from the verified token
- Use HTTPS-only in production — never transmit tokens over plain HTTP

---

*Inspired by [GitHub's security advisory process](https://docs.github.com/en/code-security) and [OWASP Responsible Disclosure](https://cheatsheetseries.owasp.org/cheatsheets/Vulnerability_Disclosure_Cheat_Sheet.html).*
