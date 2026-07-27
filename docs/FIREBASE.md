# Firebase Setup — EcoSwitch AI

> **Status: Planned — not yet implemented.**
>
> The environment variable slots (`NEXT_PUBLIC_FIREBASE_*`, `FIREBASE_ADMIN_CREDENTIAL`) are defined in `.env.example` and the client-side auth token hook exists as a stub. No Firebase SDK code is in the repository yet.

This document describes the intended Firebase integration.

---

## Planned Firebase Services

| Service | Planned Use |
|---------|-------------|
| Firebase Authentication | User sign-up, sign-in, session management |
| Firebase Firestore | Real-time energy reading storage |
| Firebase Cloud Messaging | Push notifications for anomaly alerts |
| Firebase Hosting | *(Optional)* Static frontend hosting |

---

## Planned Setup Steps

### 1. Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project: **EcoSwitch AI**
3. Enable **Google Analytics** (optional)

### 2. Register a Web App

1. In Project Settings → General → Your apps → **Add app** → Web
2. Copy the config object — map each field to `.env.local`:

```env
NEXT_PUBLIC_FIREBASE_API_KEY=AIza...
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=ecoswitch-ai.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=ecoswitch-ai
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=ecoswitch-ai.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=123456789
NEXT_PUBLIC_FIREBASE_APP_ID=1:123:web:abc
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID=G-XXXXXXXXXX
```

### 3. Enable Authentication

1. Firebase Console → Authentication → Sign-in method
2. Enable: **Email/Password** and **Google** (at minimum)

### 4. Set Up Firestore

1. Firebase Console → Firestore Database → Create database
2. Start in **test mode** for development (lock down rules before production)

### 5. Firebase Admin SDK (Server-Side)

1. Firebase Console → Project Settings → Service accounts → Generate new private key
2. Download `service-account.json`
3. **Never commit this file** — it is git-ignored
4. Base64-encode for use as an environment variable:
   ```bash
   base64 -i service-account.json | tr -d '\n'
   ```
5. Set `FIREBASE_ADMIN_CREDENTIAL` in `.env.local`

---

## Planned Integration Points

### Client Auth Hook

The stub in `lib/api-client-react/src/custom-fetch.ts` is ready to receive Firebase Auth tokens:

```typescript
// When Firebase Auth is implemented, wire it like this:
import { getAuth } from "firebase/auth";

setAuthTokenGetter(async () => {
  const user = getAuth().currentUser;
  if (!user) return null;
  return user.getIdToken();
});
```

### Server Verification

The Express API will verify Firebase ID tokens server-side using the Admin SDK:

```typescript
// Planned middleware: artifacts/api-server/src/middlewares/auth.ts
import { getAuth } from "firebase-admin/auth";

export async function requireAuth(req, res, next) {
  const token = req.headers.authorization?.replace("Bearer ", "");
  if (!token) return res.status(401).json({ error: "Unauthorized" });
  const decoded = await getAuth().verifyIdToken(token);
  req.user = decoded;
  next();
}
```

---

## Security Rules (Planned Firestore)

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can only access their own data
    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    // Device readings — write from server only (Admin SDK bypasses rules)
    match /readings/{readingId} {
      allow read: if request.auth != null;
      allow write: if false; // Server-side only
    }
  }
}
```

---

## Notes

- `NEXT_PUBLIC_` prefix is a Next.js convention for public environment variables. If the project uses Vite instead, use `VITE_` prefix and `import.meta.env.VITE_FIREBASE_*`
- Firebase Admin SDK credentials must stay server-side — never include them in the client bundle
