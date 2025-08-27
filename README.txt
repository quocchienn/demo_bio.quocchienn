
# Offline Fallback (Crazy Page) — Deploy Guide

This folder contains:
- index4.html  — main page (registers Service Worker & redirects to crazy page when offline)
- crazy-404-page.html — offline fallback page (auto-return when back online; includes "Retry" button)
- sw.js — Service Worker that returns crazy-404-page.html for failed navigations (offline)

## Local quick test
1) Serve this folder over HTTPS or localhost (Service Workers require secure context).
   - Python: `python -m http.server 8080` (http works on localhost)
2) Visit http://localhost:8080/ and open DevTools → Application → Service Workers to confirm activation.
3) Toggle Network → Offline and refresh to see the crazy page.

## Vercel
1) Put these three files at the repo root (or drag-and-drop the folder as a new project).
2) Framework preset: **Other**, Build Command: *(empty)*, Output Directory: `/`.
3) Deploy. Your site will be served via HTTPS by default.
4) Test: go to `/crazy-404-page.html` directly, then simulate offline and refresh the homepage.

## Render (Static Site)
1) New → Static Site → connect repo (or manual upload).
2) Build Command: *(empty)*, Publish Directory: `/`.
3) Deploy and test as above.

### Notes
- If your site lives under a subpath (e.g., `/bio/`), update:
  - In `index4.html`: redirect to `'/bio/crazy-404-page.html'` inside the offline handler.
  - In `crazy-404-page.html`: set the retry `location.href='/bio/'`.
  - In `sw.js`: `const OFFLINE_URL = '/bio/crazy-404-page.html';`
- If you rename `index4.html` to `index.html`, the root `/` will open automatically.
