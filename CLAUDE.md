# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static, single-page web app that displays daily QR codes for **Pines Mont Kiara** (a residential property). It shows a QR code image for the current date, a time-aware greeting, bilingual UI (English/Simplified Chinese), an interactive date picker, and a PDF house guide viewer.

## No Build System

This is a pure static site — there is no `package.json`, no bundler, no test framework. The entire app lives in a single `index.html` file. Run it by opening `index.html` directly in a browser, or serve it with any static file server:

```bash
# Quick local server (Python)
python -m http.server 8080

# Or Node
npx serve .
```

## Architecture

Everything is in `index.html`: HTML structure, inline `<style>`, and inline `<script>`. No external JS files, no modules.

**Key architectural pieces (all in the `<script>` block):**

- `config` object — GitHub username, repo name, folder, file extensions, PDF filename
- `state` object — current date and selected language (`'en'` or `'zh'`)
- `translations` dictionary — all UI strings in both languages
- Function-based flow: `initApp()` → `updateUI()` → individual update functions

**QR image loading:** Images are fetched directly from GitHub Raw Content (`raw.githubusercontent.com`) using the filename format `DDMMYYYY.jpeg`. The `updateImage()` function tries each extension in `config.fileExtensions` (`['jpeg', 'jpg']`) and falls back on error.

**Timezone:** All date logic is hardcoded to `Asia/Kuala_Lumpur`.

## Adding New QR Codes

Upload a JPEG named `DDMMYYYY.jpeg` (e.g., `17042026.jpeg`) to the repo root. The app will pick it up automatically.

## Modifying the App

All configuration is at the top of the `<script>` block:

```js
const config = {
    username: 'clement119',
    repo: 'dailyqr-bigroom-pines',
    folder: '',                         // subfolder within repo, empty = root
    fileExtensions: ['jpeg', 'jpg'],
    pdfName: 'scot-guide-CN.pdf'
};
```

To change the PDF guide, replace `scot-guide-CN.pdf` in the repo and update `config.pdfName`.

To add a new language, add an entry to the `translations` object and wire it up in `toggleLanguage()`.
