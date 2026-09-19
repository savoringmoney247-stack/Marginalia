# Marginalia — installable PDF markup PWA

Everything in this folder is flat on purpose (no subfolders), so you can drag the
whole thing into a new GitHub repo in one go.

## What's in here

- `index.html` — the app (viewer, annotation tools, export)
- `manifest.webmanifest` — makes it installable and registers it as a PDF file handler
- `sw.js` — service worker; caches the app so it keeps working offline after the first visit
- `pdf.min.js`, `pdf.worker.min.js` — PDF.js (renders pages)
- `pdf-lib.min.js` — pdf-lib (builds the exported/annotated copy)
- `icon-192.png`, `icon-512.png`, `icon-512-maskable.png`, `apple-touch-icon.png`,
  `favicon.ico`, `favicon-32.png`, `favicon-16.png` — app icons

All of it is static — no build step, no server-side code, no dependencies to install.

## Deploy it (GitHub Pages, ~2 minutes)

1. Create a new repository on GitHub (public or private).
2. Upload every file in this folder to the repo root — GitHub's web "Add file →
   Upload files" works fine since there are no subfolders to worry about, or:
   ```
   cd this-folder
   git init
   git add .
   git commit -m "Marginalia PWA"
   git branch -M main
   git remote add origin https://github.com/<you>/<repo>.git
   git push -u origin main
   ```
3. In the repo, go to **Settings → Pages**, set **Source** to the `main` branch,
   root folder, and save.
4. GitHub gives you a URL like `https://<you>.github.io/<repo>/`. Open it —
   that's the live app.

Any static host works the same way (Netlify, Vercel, Cloudflare Pages, your own
server) — just serve this folder as-is over HTTPS. HTTPS is required for the
service worker and "Install app" prompt to work; `localhost` also works for
local testing.

## Installing it as an app

Open the deployed link in Chrome, Edge, or Android Chrome and use the browser's
"Install app" prompt, or the **Install** button in the app's own header. Once
installed, opening a `.pdf` file from your file manager and choosing
"Open with → Marginalia" launches the app straight into that file (the File
Handling API — currently supported in Chromium-based browsers on desktop and
Android; other browsers just skip this and the manual **Open PDF** button
and drag-and-drop always work everywhere).

## Notes

- The two-tone teal/paper icon set was generated for this project; swap the
  PNGs for your own artwork any time — the manifest just points at these
  filenames.
- The heading/body fonts are loaded from Google Fonts over the network; if
  that request fails (e.g. fully offline on first load) the page falls back
  to the system font, so nothing breaks.
- Nothing here talks to a server: PDFs you open never leave the device, and
  the original file is only ever read, never modified — the app builds a
  brand-new file for the download.
