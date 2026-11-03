# Biodiversity Club NITC website

Files
- `index.html`  the whole website (no build step, no dependencies)
- `Code.gs`     Google Apps Script backend that uses a Google Sheet as the database

## 1. Backend (Google Sheet + Apps Script, free)
1. Create a new Google Sheet. Keep it private. Do not share or publish it.
2. Extensions > Apps Script. Paste `Code.gs` and save.
3. Run `setup()` once and approve permissions. This creates the tabs (users, sessions, passport, events) and a random secret.
4. Deploy > New deployment > Web app. Execute as: Me. Who has access: Anyone. Copy the `/exec` URL.
5. Open `index.html`, find `const CONFIG={API_URL:''}` near the top of the script, and paste the URL.
6. After any change to Code.gs, redeploy with Manage deployments > Edit > New version.

Until a URL is set the site runs in preview mode: accounts live only in the visitor's browser.

## 2. Hosting (free)
Upload `index.html` to GitHub Pages, Netlify or Vercel. Pages are reached as `/#events`, `/#members`, `/#join`, `/#me`.

## Security notes
- The sheet is only reachable through the script, which runs as you. Never share the sheet itself.
- Passwords are salted and hashed (SHA-256, 2000 rounds, plus a secret kept in Script Properties). Plain passwords are never stored.
- Sessions use random tokens; only a hash of each token is stored. Sessions expire after 14 days.
- Login is limited to 5 failed attempts per email per 15 minutes. Signup has a hidden honeypot field.
- Text is cleaned before saving so spreadsheet formulas cannot be injected.
- Use HTTPS hosting (GitHub Pages, Netlify and Vercel all do).
- Apps Script cannot check where a request comes from, so anyone with the URL can call it. Keep the URL out of public repos if that matters to you, and watch the `users` tab for odd signups.
- Members are listed in the `TEAM` object in `index.html`. Edit names, roles or add `photo` support there.
