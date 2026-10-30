# Household Water Tracker — Version 4

## What's new
- Effective-dated tariff history: old rates are retained when new rates are added.
- Mains daily usage tiers are applied progressively.
- Recycled-water rates are applied by effective date.
- Forecasts split the billing cycle across tariff effective dates.
- Actual bill reconciliation estimates usage charges only when readings cover the complete invoice period.
- Existing local data uses the same browser storage key and is migrated to schema version 4.

## Updating from Version 1–3
1. Export a JSON backup from Backup & settings.
2. Replace the repository's `index.html` with this package's `index.html` and commit.
3. Wait for Cloudflare Pages to deploy.
4. Open the same site in the same browser profile and confirm your readings and bills remain.
5. Export a fresh JSON backup after confirming the migration.

## Tariff assumptions from your uploaded Yarra Valley Water bill
The 28 August 2026 bill shows:
- 28 May–27 August 2026: mains usage 48 kL, $188.67; recycled usage 2 kL, $4.00; total usage charges $192.67.
- Before 1 July 2026: mains tier 1 $3.5724/kL; tier 2 $4.6871/kL; recycled $1.9681/kL.
- From 1 July 2026: mains tier 1 $3.8313/kL; tier 2 $5.0268/kL; recycled $2.0133/kL.
- The earlier third-tier rate is not shown on the bill. The app marks it unverified and retains $5.8403/kL from the previous configuration for the current period; verify it before relying on high-use forecasts.

Forecasts are estimates, not invoices. Small differences from actual charges can result from tariff-day allocation, meter-read timing, rounding, fixed charges, credits, concessions or adjustments.

## Multi-device data hosting
Version 4 remains local-first. Cloudflare Pages hosts the website, but it does not synchronise browser storage between phone and laptop.

Recommended architecture:
- Cloudflare Pages: private HTTPS website hosting.
- Supabase Auth: sign-in on each device.
- Supabase Postgres: readings, invoices, bills, tariff versions and settings.
- Supabase Storage: private invoice PDFs and meter photos.
- Row Level Security: each signed-in user can access only their own records.

A starter schema is provided in `supabase_schema.sql`. It uses one row per logical record, which is better suited to merging updates from two devices than overwriting one large JSON document. It also creates a private file bucket.

Cloud sync is not connected in this Version 4 build. The next step is to create a Supabase project, apply the SQL, then add sign-in and conflict-safe sync. Do not put a Supabase secret/service-role key in browser code. Use only the publishable key with tested RLS policies.
