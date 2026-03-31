# Ledger Expense Tracker

Ledger is a single-page expense tracker built with plain HTML, CSS, and JavaScript.
It is deploy-ready as a static web app and supports installable PWA behavior.

## Features

- Dashboard with monthly summaries and charts
- Add, filter, sort, and delete transactions
- Category management with custom icon and color
- Budget tracking with over-budget alerts
- CSV, printable PDF, and JSON export
- JSON import and full reset
- Offline-ready app shell via Service Worker
- Installable as a PWA on supported browsers

## Data Storage Model

Ledger stores data locally in the browser under this key:

- localStorage key: ledger_v1

Stored schema:

- version: number
- budget: number
- categories: array of category objects
- expenses: array of expense objects

Important behavior:

- Data never leaves the device unless the user exports it manually.
- Clearing browser site data deletes stored Ledger data.
- Private/incognito modes may block persistent localStorage.
- Import replaces existing data after user confirmation.

## Project Structure

- index.html: Entire app UI and logic
- manifest.webmanifest: PWA manifest
- service-worker.js: Offline caching and fetch handling
- icons/: App icons for install and mobile home screen

## Run Locally

Use any static server (required for Service Worker and full PWA behavior):

- Python: python -m http.server 8080
- Node: npx serve .

Then open:

- http://localhost:8080

## Deploy

Deploy this folder to any static host, for example:

- GitHub Pages
- Netlify
- Vercel (static)
- Cloudflare Pages

Deployment requirements:

- Serve over HTTPS (or localhost in development)
- Keep all files in the same root path
- Do not disable Service Worker or manifest files

## PWA Checklist

- Web App Manifest included
- Service Worker registered in index.html
- Offline cache for app shell and Chart.js
- Theme color and mobile app metadata configured
- App icons (192, 512, maskable, and Apple touch icon) included
