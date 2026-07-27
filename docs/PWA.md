# PWA Features — EcoSwitch AI

> **Status: Planned — no Service Worker or Web App Manifest exists yet.**
>
> Recharts, Framer Motion, and shadcn/ui are installed. The PWA shell (manifest, service worker, offline caching) is not yet implemented.

---

## Planned PWA Capabilities

| Feature | Status |
|---------|--------|
| Web App Manifest | 🔨 Planned |
| Service Worker | 🔨 Planned |
| Offline dashboard (read-only) | 🔨 Planned |
| Background sync (queue readings when offline) | 🔨 Future |
| Push notifications (FCM) | 🔨 Planned |
| Install prompt (Add to Home Screen) | 🔨 Planned |
| Lighthouse PWA score ≥ 90 | 🔨 Target (not measured yet) |

---

## Planned Implementation

### Web App Manifest (`public/manifest.json`)

```json
{
  "name": "EcoSwitch AI",
  "short_name": "EcoSwitch",
  "description": "Smart Energy Waste Reduction",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#0f172a",
  "theme_color": "#16a34a",
  "icons": [
    { "src": "/icons/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icons/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

### Service Worker (Workbox)

The planned implementation will use **Vite PWA plugin** with Workbox:

```bash
# Will be added when PWA work begins
pnpm --filter @workspace/<frontend-slug> add -D vite-plugin-pwa
```

**Planned caching strategy:**
- `StaleWhileRevalidate` for API responses (dashboard data)
- `CacheFirst` for static assets (JS, CSS, fonts, icons)
- `NetworkFirst` for real-time readings

### Offline Support

When offline, the dashboard will display the last cached readings with a clear "You are offline — showing last known data" banner. Write operations (e.g. device settings) will be queued and synced on reconnect.

---

## Push Notifications (Planned)

Push notifications will be delivered via **Firebase Cloud Messaging (FCM)**:

- Anomaly detected (usage spike > threshold)
- Device disconnected alert
- Monthly usage report ready
- Cost savings milestone reached

---

## Testing PWA

Once implemented, test with:

```bash
# Lighthouse CLI
npx lighthouse http://localhost:5173 --only-categories=pwa --output=json

# Chrome DevTools → Application → Service Workers
```

> **Honesty note:** No Lighthouse scores are claimed until the PWA is implemented and measured.
