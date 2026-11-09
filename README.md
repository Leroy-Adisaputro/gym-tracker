# Gym Log — private offline iPhone PWA

Four tabs: Gym Log, Progress (Weekly Summary / Overall), Body Weight, Data.

## Install using only Windows and an iPhone

1. Upload the **contents** of this folder to a static HTTPS host (for example GitHub Pages, Netlify, or Cloudflare Pages). Do not upload personal gym data: the site contains only app code and starter exercise templates.
2. Open the resulting HTTPS URL in **Safari on iPhone**.
3. Tap **Share → Add to Home Screen → Add**. Open the new Home Screen app once while online so its offline cache can initialize.
4. Use it offline. Data is stored in that app's IndexedDB on your iPhone.
5. In **Data**, export a JSON backup and save it to Files/iCloud Drive/OneDrive **outside the app**. Repeat at least weekly. To switch phones, install the same PWA on the new phone, then import that backup.

For Windows local testing, open a terminal in this folder and run `py -m http.server 8000`, then visit `http://localhost:8000`. A local test database is separate from your iPhone database. iPhone installation requires an HTTPS site; do not rely on a `file://` URL.

## Storage and security

- IndexedDB stores records keyed by type/date, with compact exercise IDs; graphs and summaries are computed from raw records.
- Changes are auto-saved. A workout is counted complete only when explicitly marked complete.
- The reference values from the supplied workout screenshots are **starter previous-value hints**, not fabricated dated sessions.
- No account, analytics, tracking, external API, or cloud database is used.
- Local-only storage **cannot guarantee against loss**. The backup button prepares a downloadable file, but you must save a copy off-device. The app cannot silently upload it or automatically save it to iCloud in the background.
- JSON import replaces current data only after confirmation. Export your current data before importing.
- Cardio entries are optional and only appear in graphs when a value was recorded.
- This is a first working version; the app is not an App Store native app.

## Reliability and performance revision (v10)
- Database writes are serialized, snapshot before queueing, and resolve on transaction commit. Failed writes display a warning.
- JSON/CSV exports wait for pending writes; imports validate nested data and replace records in one transaction.
- Previous-workout/PR history is computed once per workout view and invalidated after edits.
- Swipe neighbor markup is prepared at touch start; the finger-following panels no longer compute their content during touchmove.
- Service worker cleans only this app's old caches, checks for updates on returning to the app, and remains offline-first.
- No database schema or backup version changes.

### Regression checklist (manual iPhone testing)
1. Enter weights/reps rapidly, switch tabs, relaunch and verify saved values.
2. Edit steps and cardio, save and delete a weigh-in, and verify charts and weekly summary.
3. Export JSON and CSV, verify data; import JSON after confirmation, including previous settings.
4. Swipe progress/weight periods and Gym Log days; verify fixed bottom bar and vertical scrolling.
5. Open offline, then reconnect and reload after deployment.

## Review changes (v11)
- Persisted records are cloned before editing; rapid changes no longer mutate shared in-memory objects before being queued.
- Unsaved changes remain in memory after a failed IndexedDB write; a persistent Retry saving banner retries the latest snapshot. Do not close the app before retrying.
- CSV rows are built from named columns, fixing body-weight values appearing in the wrong column.
- Swipe previews are built only after horizontal intent is established, avoiding unnecessary work on vertical scroll.
- The service worker cache version is incremented and shows a reload notice after a controller update.
- Existing `day:index` exercise keys are deliberately preserved. Renaming or reordering exercises requires a separate migration to retain history.
- The original data schema, JSON backup version, icons, UI and bottom navigation are retained.


## v12 independent review
- Safer retry of failed writes using snapshot identity; avoids clearing newer pending edits.
- Deleted weigh-ins wait for outstanding writes and database confirmation.
- Validates imported backup size, activity steps, settings reminder, and exercise key ranges.
- Prevents invalid step/cardio input from being persisted.
- Swipe animation ignores completion if its view was removed.
- Service worker cache bumped and offline navigation fallback added.
- Long charts cap interactive point markers while preserving the trend polyline.
- Backup reminder now reflects its saved frequency.

Existing IndexedDB schema, exercise keys, and backup version remain unchanged.

## v13 — Rucking
- Added optional rucking steps and pack weight (lb) immediately under daily steps on Gym Log.
- Rucking is stored inside the existing activity record, exported in JSON and CSV, and shown in the weekly cardio list.
- Rucking steps are **not added** to the daily steps total to prevent double counting.
- Older records and backups without a `ruck` field remain compatible.


## v14 — Rucking charts
- Progress → Overall → Cardio now includes Rucking Steps and Pack Weight (lb) charts.
- Both charts support the existing time windows and date navigation.
- Weekly Summary displays total rucking steps and number of logged rucking days.
- Daily steps and rucking steps remain separate to prevent double counting.
- Existing records, JSON backup format, and CSV export are unchanged.
