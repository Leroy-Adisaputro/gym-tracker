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
