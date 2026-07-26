# Security Policy

## Reporting a Vulnerability

**Please do not report security vulnerabilities through public GitHub issues.**

If you discover a security vulnerability in EcoSwitch AI, we appreciate your help in disclosing it to us responsibly.

### How to Report

Send a detailed report to:

**📧 security@ecoswitch.ai** *(replace with your actual security contact)*

Please include as much of the following as possible:

- **Type of vulnerability** (e.g. SQL injection, XSS, authentication bypass, exposed credentials)
- **Location** — file path, endpoint, or component affected
- **Steps to reproduce** — a minimal, clear reproduction path
- **Proof of concept** — code snippet, curl command, or screenshot if safe to share
- **Potential impact** — what an attacker could do if this were exploited
- **Suggested fix** (optional, but appreciated)

### What to Expect

| Timeline | Action |
|----------|--------|
| **Within 48 hours** | Acknowledgement of your report |
| **Within 7 days** | Initial assessment and severity triage |
| **Within 30 days** | Patch released (for confirmed vulnerabilities) |
| **After patch** | Public disclosure coordinated with you |

We will keep you informed throughout the process and credit you in the release notes (unless you prefer to remain anonymous).

---

## Supported Versions

| Version | Supported |
|---------|-----------|
| Latest (`main`) | ✅ Yes |
| Older releases | ❌ No — please upgrade |

---

## Security Best Practices for Contributors

When contributing to this project, please follow these guidelines:

### Environment Variables & Secrets

- **Never** commit `.env`, `.env.local`, or any file containing real credentials
- Use `.env.example` to document required variables with empty values
- The `.gitignore` already excludes all common secret file patterns — do not remove those entries
- Rotate any credentials immediately if you suspect they were accidentally committed

### Dependencies

- Pin major versions and use the `catalog:` entries in `pnpm-workspace.yaml`
- Run `pnpm audit` before submitting a PR that adds or updates dependencies
- Avoid dependencies with known CVEs or that are abandoned

### Input Validation

- All API inputs must be validated with Zod schemas (generated from the OpenAPI spec)
- Never trust client-supplied data without server-side validation
- Parameterise all database queries — never concatenate user input into SQL strings

### Authentication & Authorisation

- Do not expose internal IDs in API responses where avoidable
- Always verify session tokens server-side before returning sensitive data

---

## Vulnerability Disclosure Timeline

We follow a **90-day coordinated disclosure** policy. If we cannot release a fix within 90 days, we will notify you and agree on an extension or partial disclosure.

---

*This policy is inspired by industry standards from [GitHub](https://docs.github.com/en/code-security/getting-started/adding-a-security-policy-to-your-repository) and [Responsible Disclosure](https://cheatsheetseries.owasp.org/cheatsheets/Vulnerability_Disclosure_Cheat_Sheet.html).*
