# Treasury Scout — Android PWA MVP

Treasury Scout is a phone-first Progressive Web App (PWA) for viewing U.S. Treasury marketable securities in the MVP scope:

- Bills
- Notes
- Bonds
- TIPS
- Floating Rate Notes (FRNs)
- Cash Management Bills (CMBs)

It intentionally excludes brokerage inventory, secondary-market quotes, savings bonds, tax optimization, and personalized investment recommendations.

## What changed from the Streamlit MVP

The first MVP was a Python/Streamlit app. That works well on a desktop or server, but it is awkward on Android because the phone would need a Python runtime and local server.

This version is refactored into a static mobile web app:

- `index.html`
- `styles.css`
- `app.js`
- `manifest.webmanifest`
- `service-worker.js`

There is no build step and no backend server. The app fetches public Treasury data directly from the Fiscal Data API.

## Data sources used by the app

The app calls these public Fiscal Data API endpoints:

```text
https://api.fiscaldata.treasury.gov/services/api/fiscal_service/v1/accounting/od/auctions_upcoming
https://api.fiscaldata.treasury.gov/services/api/fiscal_service/v1/accounting/od/auctions_query
```

## Local run

From this folder:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

For Android installability, deploy over HTTPS using GitHub Pages, Netlify, Vercel, or another static host.

## GitHub Pages deployment

This repo includes a GitHub Actions workflow:

```text
.github/workflows/pages.yml
```

To enable it:

1. Open the GitHub repository.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Source: GitHub Actions**.
4. Push or re-run the workflow.

The app should then publish at a URL like:

```text
https://<your-github-username>.github.io/<repo-name>/
```

For this repository, the likely URL is:

```text
https://ichimokumike-netizen.github.io/treasury-scout/
```

## Install on Android

After the app is deployed over HTTPS:

1. Open the site in Chrome or Samsung Internet on Android.
2. Tap the browser menu.
3. Choose **Install app** or **Add to Home screen**.
4. Launch **Treasury Scout** from your Android home screen.

## Files

```text
index.html                 Main app shell
styles.css                 Mobile-first styling
app.js                     Treasury API calls, filters, planner logic
manifest.webmanifest       PWA install metadata
service-worker.js          Offline shell caching
assets/icons/icon-192.png  Android install icon
assets/icons/icon-512.png  Android install icon
.github/workflows/pages.yml GitHub Pages deployment workflow
.nojekyll                  Prevents Jekyll processing on Pages
```

## Notes and limitations

- This is an educational MVP, not investment advice.
- It does not place orders or connect to TreasuryDirect.
- It does not authenticate with brokerages.
- Treasury API availability and CORS behavior must be verified from the deployed domain.
- If Treasury blocks direct browser calls from the deployed site, the next version should use a tiny serverless proxy endpoint.
