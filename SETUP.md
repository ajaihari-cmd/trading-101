# Trading 101 Workshop — Deployment Setup

To run the workshop with players on **different devices over the open internet**, you need two things:

1. **Firebase Realtime DB** so trades sync across devices.
2. **GitHub Pages** to serve the HTML files at a public URL.

Both are free. About 15 minutes total.

---

## 1 · Set up Firebase

1. Go to <https://console.firebase.google.com> and click **Add project**. Name it anything (e.g. `trading-101-workshop`). Skip Google Analytics.
2. Inside the project, click **Build → Realtime Database → Create database**. Pick a region close to your players. Start in **test mode** (rules will allow anyone to read/write — fine for a workshop, do not use for real apps).
3. Click the gear icon → **Project settings**.
4. Scroll to **Your apps** → click the **&lt;/&gt;** web icon → register a web app (any nickname).
5. Copy the `firebaseConfig` object Firebase shows you.
6. Open `firebase-config.js` in this folder and paste the values in. Make sure `databaseURL` is the one ending in `.firebaseio.com` — Firebase sometimes omits it; if so, find it under **Realtime Database** in the console.

That's it for Firebase. To wipe state between rehearsals, delete the `world` and `teams` nodes from the Realtime DB console.

---

## 2 · Push to GitHub Pages

1. Create a new public GitHub repo (e.g. `trading-101`).
2. Upload these files at the repo root (or any subfolder):
   - `workshop-game.html` — your (instructor) dashboard
   - `workshop-participant.html` — what players open
   - `join.html` — the QR code page you'll project on screen
   - `firebase-config.js` — your filled-in config
3. In the repo, go to **Settings → Pages**. Source: `main` branch, root folder. Save.
4. After a minute, GitHub gives you a URL like `https://YOUR-USERNAME.github.io/trading-101/`.

---

## 3 · Wire the QR code

Open `join.html` and edit the `PARTICIPANT_URL` constant near the bottom to point at:

```
https://YOUR-USERNAME.github.io/trading-101/workshop-participant.html
```

Commit and push. Reload `join.html` — the QR code updates.

---

## 4 · (Optional) Shorten with bitly

1. Go to <https://bitly.com>, sign up (free).
2. Paste your participant URL → get a short link like `https://bit.ly/trading101`.
3. You can either (a) leave the QR pointing at the long URL or (b) edit `PARTICIPANT_URL` in `join.html` to the bit.ly link so the QR is denser-but-shorter.

---

## 5 · Run the workshop

- Open `https://YOUR-USERNAME.github.io/trading-101/workshop-game.html` on your projector laptop.
- Open `https://YOUR-USERNAME.github.io/trading-101/join.html` on a second tab and show it on screen so players can scan.
- Players scan, name their team, start trading. As they trade, their cards appear on your right-side panel ranked by MTM P&amp;L.
- Hit **Next round** to advance the storyline.

---

## Local-only testing (no Firebase)

If `firebase-config.js` still has the placeholder values, both pages fall back to `localStorage` + `BroadcastChannel`. That works only across **tabs in the same browser on the same Mac** — fine for solo testing. Just open `workshop-game.html` in one tab and `workshop-participant.html` in another tab (or several).
