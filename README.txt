# Household Water Tracker — Versions 1–3 feedback build

## Run locally
Open `index.html` in a modern browser. The app stores records in that browser's local storage; it does not sync between devices. PDF text extraction uses PDF.js from a CDN, so an internet connection is needed for automatic text extraction. PDF content is parsed in the browser and is not uploaded by the app. Photos/scanned bills can be previewed, but OCR is not included; enter those values manually.

## Deploy to Cloudflare Pages
1. Create a private GitHub repository and upload `index.html` to its root.
2. In Cloudflare, create a Pages project connected to that repository.
3. Set the repository root as the output directory; no build command is required.
4. Protect the deployed hostname with Cloudflare Access. A private source repository does not, on its own, make the deployed site private.
5. Before replacing files or changing browser/device, export a JSON backup. The app remains local-first and does not synchronise data across devices.

## Data and forecast behaviour
- Invoice imports create/link meter observations at the period start and end and save the actual invoice record.
- Re-importing a previously saved invoice is checked using invoice number and billing period/total; conflicting readings are rejected instead of silently overwritten.
- If bills are imported out of chronological order, the provisional next cycle is based on the latest bill by billing-period end date rather than the last bill imported.
- If no readings overlap a selected billing cycle, the dashboard uses the latest available measured consumption rate to produce a labelled trend-based estimate instead of displaying a false $0 forecast. If only part of a cycle is covered by readings, uncovered days are estimated from recent usage.
- Forecasts are estimates, not invoices. Rate settings currently apply one active tier structure to the forecast; exact historic tariff segmentation is retained as bill text but is not yet a full effective-date tariff engine.
- Chart choices (period, metric and date range) are saved locally. Cumulative meter totals remain selected when changing period granularity; period granularity is only applied to consumption charts.
- After a successful invoice import, the entry form is cleared and a success message remains visible.
- Chart symbols: actual readings are solid dots, calculated points are dotted-line-connected dots, and forecasts are stars. Missing consumption periods appear as gaps.

## Privacy and storage
- All records remain in this browser unless exported. PDF.js is downloaded from a CDN; the bill text itself is processed in the browser.
- Clear browser storage only after exporting a backup. The app does not currently offer encrypted cloud backup or multi-device sync.
