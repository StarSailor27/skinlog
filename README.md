# SkinLog — daily face log (PWA)

A lean, local-only web app to track your face day-to-day: capture **front /
left 45° / right 45°** snapshots with an eyes-nose-mouth alignment guide, then
review them as a **daily album grid** and a **same-angle timeline slider**.
No AI analysis, no accounts, no cloud — a simple replacement for the defunct
"스킨로그" daily-log use case.

## Privacy

Photos are stored **only on the device**, in the browser's IndexedDB. Nothing
is uploaded anywhere. Clearing the site data / browser storage deletes them, so
this is a personal log, not a backup. (Export can be added later if wanted.)

## Features

- **촬영 (Capture):** live selfie camera with a **real-time 3D face mesh**
  (MediaPipe Face Landmarker, 478 landmarks) overlaid on your face, plus an
  angle readout (front / left 45 / right 45) that turns green when your head
  matches the selected angle. One shot per angle per day; re-shooting the same
  angle overwrites that day's slot. Green dots show which angles are done today.
  - The mesh model loads from a CDN on first run (needs network once, then the
    browser caches it). **If it can't load (offline first run), the app falls
    back to a static 2D face-outline guide** — capture still works.
- **앨범 (Album):** newest-first, grouped by date, 3 columns (front/left/right).
  Tap a photo to view full-screen or delete it.
- **비교 (Compare):** pick an angle, drag the slider to scrub that angle across
  all dates — the before/after timeline.

## Run

Camera access requires **HTTPS or localhost**. Serve the folder:

```bash
cd /Users/bsyoo/Projects/hermes-pando/tools/skinlog
python3 -m http.server 8765
# then open http://localhost:8765 on this machine
```

### On your phone (the intended use)

The camera needs a secure context, so plain `http://<mac-ip>:8765` from the
phone will **not** grant camera access. Options:

1. **Host it** on any static HTTPS host (GitHub Pages, Netlify, Vercel, Cloudflare
   Pages) — drop these files in, open the https URL on the phone, then
   **Share → Add to Home Screen** to install as an app. (Photos still stay local
   to the phone.)
2. **Local HTTPS tunnel** (e.g. `cloudflared tunnel --url http://localhost:8765`)
   gives a temporary https URL you can open on the phone.
3. iOS/Android both support "Add to Home Screen" → it then runs full-screen like
   a native app and works offline (service worker caches the shell).

## Files

- `index.html` — the whole app (UI + camera + IndexedDB + guides), no dependencies.
- `manifest.json`, `sw.js`, `icon.svg` — PWA install + offline shell.

## Notes / possible next steps

- Add photo **export/import** (zip) for backup/transfer between devices.
- Optional simple notes per day (e.g. breakouts, products).
- Side-by-side two-date compare (currently single-image scrub).
