# Maintenance Visit Report

Offline-capable web app (PWA) for AV maintenance visits: rooms, devices picked from an editable library, asset details, check statuses, fault details, photos, sign-off, and a printable client report saved to Google Drive.

## Files
- `index.html` – the app (no external libraries; works offline)
- `config.js` – shared Google settings (client ID, Drive folder) for everyone using this copy
- `sw.js` – service worker for offline use
- `manifest.webmanifest`, `icons/` – lets Android/Chrome install it as an app

## 1. Publish on GitHub Pages
1. Create a repository and upload all files, keeping the `icons` folder.
2. Settings > Pages > Source: Deploy from a branch > `main`, folder `/ (root)` > Save.
3. Note the address: `https://<username>.github.io/<repo>/`. The origin is `https://<username>.github.io`.

## 2. Google Cloud: create the OAuth client ID
1. Go to https://console.cloud.google.com and create a project (or reuse an existing one).
2. APIs & Services > Library > enable **Google Drive API**.
3. Set up the OAuth consent screen (Google Auth Platform):
   - App name and support email.
   - Audience: **Internal** if everyone uses the company Google Workspace; otherwise **External**, leave it in **Testing**, and add each colleague's Google account as a test user.
   - Data access / scopes: add `https://www.googleapis.com/auth/drive`.
4. Clients (Credentials) > Create client > **Web application**.
   - Authorised JavaScript origins: `https://<username>.github.io` (no path, no trailing slash).
   - Redirect URIs: not needed.
5. Copy the **Client ID** (ends in `.apps.googleusercontent.com`).

## 3. Shared Drive folder
1. Create a folder in Google Drive (or a Shared drive) for reports.
2. Share it with colleagues as **Editor**.
3. Copy the folder link; the ID is the part after `/folders/`.

## 4. Put the settings in config.js
Fill in `googleClientId` and `driveFolderId`, commit, and bump the version in the first line of `sw.js`. Alternatively each person can enter them in the app under Settings.

## 5. First use on each device
Open the app > Settings > **Test connection** > sign in with Google > allow Drive access. Then use **Save to Google Drive** inside a visit or on the report. It uploads the report (.html) and a backup (.json, includes photos); saving again updates the same two files.

## Updating the app
After changing any file, change `mvr-v2` in `sw.js` to the next number so installed copies pick up the update.

## Where data lives
Visits are stored in the browser's IndexedDB on that device until saved to Drive or exported as a backup file. Nothing is stored on GitHub.
