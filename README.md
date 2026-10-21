# Speedo Drive — setup guide (Android-friendly)

## What's included
- `index.html`: complete neon-green/black typing racer; local bot race, WPM, accuracy, timer, results and device-local personal stats.
- `firebase-config.js`: paste your Firebase Web App config here to enable the online room lobby.
- `database.rules.json`: example Firebase Realtime Database rules.
- `manifest.webmanifest` and `sw.js`: installable/PWA basics.

## 1) Run a local race
You can test the HTML by opening it in an editor that supports preview, but ES modules and the service worker require a website served over HTTPS or localhost. The simplest reliable method is to deploy the files to a static host.

## 2) Deploy from your Android phone with GitHub Pages
1. Create/sign in to GitHub in your browser.
2. Create a new public repository named `speedo-drive`.
3. Upload all files from this folder, keeping them at the repository root.
4. Open the repository's **Settings → Pages**.
5. Under Build and deployment, choose **Deploy from a branch**, branch `main`, folder `/ (root)`, then Save.
6. Wait for the Pages URL to appear. Open it on your phone. Use browser menu → **Add to Home screen** to make an app-like shortcut.

Do not upload private credentials. Firebase web config is designed to be present in a web app, but access must be protected by Firebase Authentication and database rules.

## 3) Configure Firebase for the online lobby
Online room sharing is not active until Firebase is configured. The included lobby currently syncs room membership; it is not yet a synchronized live race engine.

1. Open https://console.firebase.google.com/ and create a project called `speedo-drive`.
2. In Project settings, add a **Web app** and copy its config values.
3. In Firebase, open **Build → Realtime Database**, create a database and choose a nearby region.
4. Copy the exact database URL Firebase displays into `databaseURL` in `firebase-config.js`.
5. Replace every `PASTE_...` placeholder in `firebase-config.js` with your actual config values.
6. In Firebase Authentication, enable an appropriate sign-in provider (Email/Password or Anonymous if available in your project).
7. Important: the included example rules require `auth != null`, so players must be authenticated before writes. The starter front end does not yet implement sign-in, so the online lobby will fail writes until authentication is added. Do not make the database publicly writable to get around this.
8. Publish the updated files to your static host.

### Security warning
Do **not** use public write rules such as `.write: true` for a deployed multiplayer game. That allows strangers to overwrite rooms and can expose users to abuse. For a real public game, add Firebase Auth, server-side validation (Cloud Functions or another trusted backend), rate limiting, and rules that restrict each player to their own record. Room codes are not security.

## 4) What “online multiplayer” means in this starter
The lobby can connect to Firebase and subscribe to room membership after configuration and authentication are implemented. This starter does **not** yet synchronize race starts, typed progress, finishes, or a competitive leaderboard between players. Those need a trusted game-state design and anti-cheat checks; don't advertise this build as a complete multiplayer racer yet.

## 5) Quick troubleshooting
- If it looks unstyled or the page is blank, confirm all files are uploaded and open the HTTPS website URL.
- If the online panel says Firebase is not configured, check `firebase-config.js` for `PASTE_` values.
- If the room write is denied, authentication/rules are not configured. Do not switch to public write access.
- If the browser does not install it, open the deployed HTTPS page and use the browser menu → Add to Home screen.
