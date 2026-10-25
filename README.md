# Household Water Tracker — Versions 1–3

## Run locally
Open `index.html` in a modern browser. The app stores records in that browser's local storage; it does not sync between devices. PDF text extraction uses PDF.js from a CDN, so an internet connection is needed for the automatic PDF import. The PDF's contents are parsed in the browser; they are not uploaded by the application. Image invoices can be attached for preview, but OCR is not included and the fields need manual entry.

## Deploy to Cloudflare Pages
1. Create a private GitHub repository.
2. Upload `index.html` to the root of the repository.
3. In Cloudflare, create a Pages project connected to that repository.
4. Configure the repository root as the output directory; no build command is required.
5. To make it private, protect the deployed hostname using Cloudflare Access. A private Git repository alone does not make the public website private.
6. After every update, use JSON export to back up the current app data before replacing files or changing browsers.

## Data notes
- Bill import adds a bill record and meter observations at the bill period's start and end after you confirm the review form.
- The next billing period is provisional, projected from the bill period length; the next meter-reading window is stored separately.
- Tariff details extracted from invoices are retained with the bill as historical text. Verify extracted values before relying on forecasts.
- This build uses browser local storage and is not a cloud-synchronised multi-device app.
