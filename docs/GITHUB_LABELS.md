# GitHub Labels — EcoSwitch AI

This document defines the label system used for issues and pull requests. Apply labels consistently to keep the tracker clean and searchable.

---

## Type Labels

| Label | Colour | Description |
|-------|--------|-------------|
| `bug` | `#d73a4a` (red) | Something isn't working correctly |
| `enhancement` | `#a2eeef` (teal) | New feature or improvement |
| `documentation` | `#0075ca` (blue) | Documentation additions or corrections |
| `question` | `#d876e3` (purple) | Further information is requested |
| `refactor` | `#e4e669` (yellow) | Code restructuring without behaviour change |

## Priority Labels

| Label | Colour | Description |
|-------|--------|-------------|
| `priority: critical` | `#b60205` (dark red) | Blocker — must be resolved immediately |
| `priority: high` | `#e11d48` (rose) | Important — target for current milestone |
| `priority: medium` | `#f97316` (orange) | Normal priority |
| `priority: low` | `#94a3b8` (slate) | Nice to have |

## Area Labels

| Label | Colour | Description |
|-------|--------|-------------|
| `area: api` | `#16a34a` (green) | Express API server |
| `area: database` | `#0891b2` (cyan) | Drizzle schema and migrations |
| `area: frontend` | `#7c3aed` (purple) | React frontend / UI |
| `area: iot` | `#ea580c` (orange) | ESP32 firmware and IoT layer |
| `area: firebase` | `#f59e0b` (amber) | Firebase Auth / Firestore / FCM |
| `area: pwa` | `#5a0fc8` (purple) | Service Worker and PWA features |
| `area: ci` | `#334155` (slate) | GitHub Actions and build pipeline |
| `area: docs` | `#0284c7` (sky) | Documentation (all files) |
| `area: security` | `#b91c1c` (red) | Security-related changes |

## Status Labels

| Label | Colour | Description |
|-------|--------|-------------|
| `status: needs triage` | `#e2e8f0` (light) | Awaiting initial review |
| `status: needs repro` | `#fef08a` (yellow) | Bug — needs reproduction steps |
| `status: in progress` | `#86efac` (green) | Being actively worked on |
| `status: blocked` | `#fca5a5` (red) | Cannot proceed — waiting on something |
| `status: on hold` | `#94a3b8` (grey) | Paused — not a current priority |
| `status: wont fix` | `#64748b` (grey) | Closed — will not be addressed |

## Contribution Labels

| Label | Colour | Description |
|-------|--------|-------------|
| `good first issue` | `#7957d5` (violet) | Suitable for first-time contributors |
| `help wanted` | `#008672` (teal) | Extra attention / help is needed |
| `needs review` | `#bfdbfe` (light blue) | PR is ready for review |

## Milestone / Release Labels

| Label | Colour | Description |
|-------|--------|-------------|
| `milestone: v0.2.0` | `#16a34a` | Tracked for v0.2.0 (data layer) |
| `milestone: v0.3.0` | `#16a34a` | Tracked for v0.3.0 (auth) |
| `milestone: v0.4.0` | `#16a34a` | Tracked for v0.4.0 (dashboard) |
| `milestone: v0.5.0` | `#16a34a` | Tracked for v0.5.0 (IoT) |

---

## Creating Labels via GitHub CLI

```bash
# Install GitHub CLI: https://cli.github.com/
gh label create "area: api"       --color "16a34a" --description "Express API server"
gh label create "area: database"  --color "0891b2" --description "Drizzle schema and migrations"
gh label create "area: frontend"  --color "7c3aed" --description "React frontend / UI"
gh label create "area: iot"       --color "ea580c" --description "ESP32 firmware and IoT layer"
gh label create "area: firebase"  --color "f59e0b" --description "Firebase Auth / Firestore / FCM"
gh label create "area: pwa"       --color "5a0fc8" --description "Service Worker and PWA features"
gh label create "good first issue" --color "7957d5" --description "Suitable for first-time contributors"
gh label create "help wanted"     --color "008672" --description "Extra attention / help is needed"
```

---

## Label Usage Guidelines

- Every issue should have: one **type** label + one **area** label + one **priority** label
- PRs should have: a **type** label + the relevant **area** label
- Add `good first issue` to anything that doesn't require deep context
- Do not add `priority: critical` to more than 2-3 issues at once — it loses meaning
