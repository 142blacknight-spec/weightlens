# WeightLens 📊

A progressive web app (PWA) for weight tracking with Health Connect integration and advanced analytics.

**Live app:** https://142blacknight-spec.github.io/weightlens/

---

## Features

- **Health Connect integration** — reads/writes weight records directly from Android Health Connect (works inside a TWA)
- **CSV import/export** — load and save data in the native format (Date, Weight kg, Fat mass, Bone mass, Muscle mass, Hydration)
- **Bi-directional sync** — when connecting Health Connect after loading a CSV, missing records are automatically written back to Health Connect
- **Interactive chart** — raw data, linear fit, and moving average layers with touch/mouse cursor inspection
- **Trend analytics** — linear fit slope displayed as kg/week, kg/month, and kg/year
- **Navigation** — step through time windows with ← prev / next → arrows
- **Moving average window** — configurable 1W / 1M / 3M smoothing window using actual date ranges
- **Unit selector** — switch between kg and lbs
- **Offline-ready** — service worker caches the full app shell

---

## Repository structure

```
weightlens/
├── index.html          ← Main app (single-file PWA)
├── manifest.json       ← Web App Manifest
├── sw.js               ← Service Worker
├── icon-192.png        ← App icon 192×192
├── icon-512.png        ← App icon 512×512
├── _config.yml         ← GitHub Pages config (includes .well-known)
├── .well-known/
│   └── assetlinks.json ← Digital Asset Links for TWA
└── README.md
```

---

## Deploying to GitHub Pages

1. Push all files to the `main` (or `master`) branch of this repository.
2. Go to **Settings → Pages** and set the source to **Deploy from branch → main → / (root)**.
3. After a minute the app is live at `https://142blacknight-spec.github.io/weightlens/`.

---

## Using with Android Health Connect

The app fully integrates with Health Connect **only when running inside a Trusted Web Activity (TWA)**.  
In a regular browser, you can still use CSV import/export.

### Quick TWA setup (Bubblewrap)

```bash
npm install -g @bubblewrap/cli
bubblewrap init --manifest https://142blacknight-spec.github.io/weightlens/manifest.json
bubblewrap build
```

Then install the generated `.apk` / upload the `.aab` to the Play Store.

### Android Manifest permissions needed

```xml
<uses-permission android:name="android.permission.health.READ_WEIGHT" />
<uses-permission android:name="android.permission.health.WRITE_WEIGHT" />
```

### Digital Asset Links

Before publishing the TWA, replace `REPLACE_WITH_YOUR_KEYSTORE_SHA256_FINGERPRINT` in  
`.well-known/assetlinks.json` with the actual SHA-256 fingerprint of your signing key:

```bash
keytool -list -v -keystore your-keystore.jks | grep "SHA256:"
```

---

## CSV format

The native export format is compatible with Withings / similar smart scales:

```csv
Date,"Weight (kg)","Fat mass (kg)","Bone mass (kg)","Muscle mass (kg)","Hydration (kg)",Comments
"2026-04-25 08:11:51",80.90,16.02,3.22,61.65,43.93,
```

---

## Tech stack

Pure HTML / CSS / JS — no build step, no framework, no dependencies.  
Fonts: [DM Mono](https://fonts.google.com/specimen/DM+Mono) + [Syne](https://fonts.google.com/specimen/Syne) via Google Fonts.
