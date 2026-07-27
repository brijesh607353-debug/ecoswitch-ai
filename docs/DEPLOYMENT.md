# Deployment Guide — EcoSwitch AI

> This guide covers deploying the actual implemented components: the **Express API server** and the **PostgreSQL database**. Firebase and PWA deployment sections describe the planned approach.

---

## What Can Be Deployed Now

| Component | Deploy-ready? | Notes |
|-----------|--------------|-------|
| Express API server | ✅ Yes | Builds to a single CJS bundle via esbuild |
| PostgreSQL database | ✅ Yes | Any managed Postgres service |
| React frontend | 🔨 Planned | UI sandbox is dev-only; production frontend not yet built |
| Firebase services | 🔨 Planned | Not yet integrated |

---

## API Server Deployment

### Build

```bash
pnpm --filter @workspace/api-server run build
# Outputs a CJS bundle to artifacts/api-server/dist/
```

### Required Environment Variables (Production)

```env
NODE_ENV=production
PORT=8080
DATABASE_URL=postgresql://user:pass@host:5432/ecoswitch
SESSION_SECRET=<random 32-byte hex string — generate with: openssl rand -hex 32>
```

### Platform Options

#### Railway / Render / Fly.io (recommended for quick deploys)

1. Connect your GitHub repository
2. Set the build command: `pnpm install && pnpm --filter @workspace/api-server run build`
3. Set the start command: `node artifacts/api-server/dist/index.js`
4. Add all required environment variables in the platform dashboard
5. Provision a managed PostgreSQL addon

#### Docker

```dockerfile
FROM node:20-alpine AS base
RUN corepack enable && corepack prepare pnpm@latest --activate

FROM base AS deps
WORKDIR /app
COPY package.json pnpm-workspace.yaml pnpm-lock.yaml ./
COPY artifacts/api-server/package.json ./artifacts/api-server/
COPY lib/api-zod/package.json ./lib/api-zod/
COPY lib/db/package.json ./lib/db/
RUN pnpm install --frozen-lockfile --prod

FROM base AS builder
WORKDIR /app
COPY . .
RUN pnpm install --frozen-lockfile
RUN pnpm --filter @workspace/api-server run build

FROM node:20-alpine AS runner
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY --from=builder /app/artifacts/api-server/dist ./dist
EXPOSE 8080
CMD ["node", "dist/index.js"]
```

#### VPS (Ubuntu/Debian)

```bash
# Install Node.js 20 + pnpm
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
npm install -g pnpm

# Clone and build
git clone https://github.com/brijesh607353-debug/ecoswitch-ai.git
cd ecoswitch-ai
pnpm install --frozen-lockfile
pnpm --filter @workspace/api-server run build

# Run with PM2
npm install -g pm2
pm2 start artifacts/api-server/dist/index.js --name ecoswitch-api
pm2 save && pm2 startup
```

---

## Database Setup (Production)

### Managed Services (recommended)

- [Neon](https://neon.tech/) — serverless Postgres, free tier available
- [Supabase](https://supabase.com/) — Postgres + extras
- [Railway Postgres](https://railway.app/) — simple addon
- [Amazon RDS](https://aws.amazon.com/rds/) — enterprise-grade

### Schema Migration

```bash
# Push schema to production database
DATABASE_URL=<your-prod-url> pnpm --filter @workspace/db run push

# Or use Drizzle Kit migrations (recommended for production):
pnpm --filter @workspace/db run generate  # generate migration files
pnpm --filter @workspace/db run migrate   # apply migrations
```

---

## Planned: Firebase Deployment

> Not yet applicable — Firebase is not integrated. See [FIREBASE.md](./FIREBASE.md).

---

## Planned: Frontend Deployment

> The production frontend does not exist yet. When built, it will be a static Vite build deployable to Vercel, Netlify, or Firebase Hosting.

---

## Health Check

Verify the deployment is working:

```bash
curl https://your-production-domain.com/api/healthz
# Expected: {"status":"ok"}
```

---

## Checklist Before Going Live

- [ ] `NODE_ENV=production` is set
- [ ] `SESSION_SECRET` is a cryptographically random value
- [ ] `DATABASE_URL` points to the production database
- [ ] Database schema has been pushed/migrated
- [ ] HTTPS is configured (TLS certificate)
- [ ] CORS origin list is restricted to your frontend domain
- [ ] No `.env.local` or secret files in the deployed bundle
- [ ] Health check endpoint returns `200 OK`
