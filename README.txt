# ADM Enrollment Live Dashboard — Netlify Package

This package contains the dashboard and a small Netlify Function that retrieves the published Google Sheet CSV from the server side. The browser calls the same Netlify site, which avoids the common `file://` / Safari CORS failure.

## Included files

- `index.html` — dashboard page with embedded snapshot fallback
- `netlify/functions/sheet-data.js` — live Google Sheets data bridge
- `netlify.toml` — tells Netlify where the site and function are located

## Before deploying

The source worksheet must remain published to the web as CSV. This package is configured for the published worksheet `gid=2063973395` supplied for the ADM Enrollment SY 2026–2027 dashboard.

## Recommended deployment: GitHub + Netlify (no Code.gs)

### A. Upload the package to GitHub

1. Extract `ADM_Enrollment_Live_Dashboard_Netlify.zip` on your laptop.
2. Sign in to GitHub and create a **New repository**. Name it `adm-enrollment-live-dashboard`.
3. Set the repository to **Private** or **Public** (either works for this dashboard).
4. Choose **Add file → Upload files**.
5. Upload the extracted package contents — make sure `index.html`, `netlify.toml`, and the `netlify` folder are at the repository's top level. Do not upload the ZIP itself as the only file.
6. Click **Commit changes**.

### B. Connect the repository to Netlify

1. Sign in to Netlify.
2. Choose **Add new site → Import an existing project**.
3. Choose **GitHub**, authorize Netlify if prompted, and select `adm-enrollment-live-dashboard`.
4. In the deployment settings, use:
   - **Branch:** `main` (or the default branch shown by GitHub)
   - **Build command:** leave blank
   - **Publish directory:** `.`
   - **Functions directory:** `netlify/functions` (if Netlify asks)
5. Click **Deploy**.

### C. Test the live connection

1. Open the Netlify site URL after deployment.
2. The dashboard should load with its embedded snapshot first, then attempt a live refresh.
3. To test the bridge directly, open this path on your deployed site:
   `https://YOUR-SITE-NAME.netlify.app/.netlify/functions/sheet-data`
4. A successful response is JSON containing `csv` and `updatedAt`. If it reports an error, check that the Google worksheet is still published to the web and that the publication URL is accessible.
5. Return to the dashboard. The status line should change to `LIVE` when the live data is read successfully. It automatically checks every two minutes; the Refresh button also triggers a check.

## Updating the dashboard later

- Changes made to the published Google Sheet should be picked up by the dashboard on its next refresh (up to about two minutes).
- If you change the published worksheet or its `gid`, update `GOOGLE_CSV_URL` in `netlify/functions/sheet-data.js`, commit the change, and let Netlify redeploy.
- The dashboard keeps its embedded snapshot as a fallback if the live source cannot be reached.

## Important notes

- This package is prepared for deployment; it is **not yet deployed** to your Netlify account.
- Do not use Netlify Drop/drag-and-drop for this package if you need the live data bridge. A static-only upload may publish the page but will not deploy the serverless function. Use the GitHub import steps above.
- No Google Apps Script / `Code.gs` is required.
