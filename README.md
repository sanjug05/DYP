# Digital Yellow Papers — Setup Guide

## 1. Firebase Project

1. Go to https://console.firebase.google.com and create a project.
2. Enable **Authentication → Email/Password**.
3. Enable **Firestore Database** (start in production mode).
4. Enable **Storage**.
5. Copy your web app config from **Project Settings → Your apps → Web app**.

## 2. Add Your Config

Open **index.html** and **admin.html**. Find `FIREBASE_CONFIG` at the top of each `<script>` block and replace all `YOUR_*` values:

```js
const FIREBASE_CONFIG = {
  apiKey:            "AIzaSy...",
  authDomain:        "your-project.firebaseapp.com",
  projectId:         "your-project",
  storageBucket:     "your-project.appspot.com",
  messagingSenderId: "12345678",
  appId:             "1:12345:web:abc123"
};
```

## 3. AQI (Optional)

Get a free token at https://aqicn.org/api/ and set it in **index.html**:

```js
const API_CONFIG = { waqi: "your-token-here" };
```

Weather works immediately with no key (Open-Meteo). News works immediately (RSS proxy).

## 4. Deploy Security Rules

```bash
npm install -g firebase-tools
firebase login
firebase init          # select Firestore + Storage + Hosting
firebase deploy --only firestore:rules,storage
```

## 5. Create Your First Admin

1. Sign up normally in the app.
2. In Firebase Console → Firestore → `users` collection, find your document.
3. Set the `role` field to `"admin"`.
4. Open `admin.html` and sign in with those credentials.

## 6. Seed Firestore (Optional)

The app works with static demo data by default. To push seed data:

```bash
node seed.js   # (seed script not included — export AppData from the HTML and adapt)
```

## 7. Run Locally

```bash
firebase serve   # or just open index.html in a browser (Firebase will work when deployed)
```

## Collections Used

| Collection | Purpose |
|---|---|
| `users` | Auth profiles + roles |
| `businesses_live` | Approved public listings |
| `business_submissions` | Pending / reviewed new listings |
| `business_edits_pending` | Pending retailer edits |
| `reviews` | User reviews |
| `local_posts` | Community posts |
| `categories` | Category list |
| `areas` | Area list |
| `sponsored_slots` | Ad/promo placements |
| `audit_logs` | Admin action log |

## Files

- `index.html` — Main app (mobile-first)
- `admin.html` — Admin dashboard (desktop)
- `firestore.rules` — Firestore security rules
- `storage.rules` — Firebase Storage security rules
