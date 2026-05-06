# SCAT6 Digital — iOS Home Screen App

A self-contained Progressive Web App (PWA) of the SCAT6. Once installed on your iPhone home screen, it runs full-screen like a native app, works fully offline, and saves all assessments locally on your device. No login, no cloud, no subscription.

---

## What's in this folder

```
scat6-app/
├── index.html                  ← the app
├── manifest.webmanifest        ← tells iOS this is an installable app
├── sw.js                       ← service worker (offline support)
└── icons/
    ├── icon-180.png            ← iOS home screen icon
    ├── icon-192.png            ← Android / general
    ├── icon-512.png            ← high-res
    └── icon-512-maskable.png   ← Android adaptive icons
```

You upload all of these together. They must stay in this layout.

---

## Step 1 — Host on GitHub Pages (~5 minutes)

GitHub Pages is free and gives you a permanent URL. You only do this once.

### 1a. Make a GitHub account (skip if you have one)

Go to **github.com** → Sign up. Free tier is fine.

### 1b. Create a new repository

1. Click the **+** in the top-right → **New repository**
2. Repository name: `scat6` (or whatever you like — this becomes part of your URL)
3. Set it to **Public** (Pages requires public on the free plan)
4. Tick **"Add a README file"**
5. Click **Create repository**

### 1c. Upload the app files

1. On your new repo's page, click **Add file** → **Upload files**
2. Drag the entire contents of the `scat6-app` folder into the upload area:
   - `index.html`
   - `manifest.webmanifest`
   - `sw.js`
   - the `icons` folder (drag the whole folder in — GitHub will preserve the structure)
3. At the bottom, write a commit message like "Initial upload" and click **Commit changes**

### 1d. Turn on Pages

1. In your repo, click the **Settings** tab (top-right of the repo page)
2. In the left sidebar, click **Pages**
3. Under **Source**, choose **Deploy from a branch**
4. Branch: **main**, folder: **/ (root)** → click **Save**
5. Wait ~1–2 minutes. Refresh the Pages settings page.
6. You'll see a green box: **"Your site is live at https://yourusername.github.io/scat6/"**

That URL is your app. Test it in any browser — you should see the SCAT6 library page.

---

## Step 2 — Install on your iPhone

This is the bit that turns the URL into a home screen app.

1. Open **Safari** on your iPhone (must be Safari — Chrome on iOS doesn't support PWA install properly)
2. Go to `https://yourusername.github.io/scat6/`
3. Tap the **Share** button (square with up-arrow at the bottom)
4. Scroll down and tap **Add to Home Screen**
5. The name will pre-fill as "SCAT6". Tap **Add** (top-right)

Done. You'll have a SCAT6 icon on your home screen. Tap it — it opens full-screen, no Safari URL bar, looks like a real app.

### After install: works offline

The first time you open it from the home screen while you have signal, it caches everything. After that, it works with no internet — perfect for pitchside or in a clinic basement with no signal. Saved assessments stay on the device permanently (localStorage).

---

## Step 3 — Day-to-day use

### Creating an assessment
- Tap **+ New Assessment** on the library screen
- Fill in as you go. Everything autosaves about half a second after each change.
- Tap **← All** in the top bar to go back to the library

### Multiple athletes
- Each assessment is its own entry in the library, sorted by most recently edited
- The library shows athlete name, exam date, team, and a colour-coded badge (Yes / No / Deferred / In progress)

### Exporting a completed assessment as PDF
1. Open the assessment
2. Tap **Export PDF** in the top bar
3. The iOS print sheet appears. Pinch out on the preview to switch to the PDF preview, OR just tap **Share** on the print sheet
4. Choose **Save to Files** (or AirDrop, email, your EHR app, etc.)

### Deleting an assessment
- From the library: tap the small **Delete** button on a card
- From inside an assessment: scroll to the bottom of Step 6 and tap **Delete this assessment**

---

## Updating the app later

If you want to change something (e.g. tweak a field, fix a typo), edit the file locally, then:

1. On your repo page, navigate to the file → click the pencil icon → paste the changes → commit
2. Bump the cache version inside `sw.js` (change `scat6-v1` to `scat6-v2`) so the service worker forces a refresh on existing installs
3. On your iPhone, fully close the app (swipe up in the app switcher and flick it off-screen), then reopen — it'll pull the new version

---

## A few notes on data safety

- **Everything is local to your iPhone.** No data leaves the device. If you delete the app from your home screen, the saved assessments go with it.
- **Periodically export important assessments to PDF** and store them properly (your private practice records, club EHR, etc.) — don't rely on the app as your archive of record.
- **iOS may clear PWA data** if you don't open the app for a long stretch (rare, but possible under storage pressure). The PDF export is your durable record.
- **This is a clinical tool** — treat it like the paper SCAT6. The same caveat from the original applies: scoring should not be used as a stand-alone method to diagnose concussion or make return-to-play decisions.

---

## Troubleshooting

**"Add to Home Screen" doesn't appear** → You're not in Safari. iOS only supports PWA install from Safari, not Chrome/Firefox/Brave on iPhone.

**The app won't load offline** → Open it once with signal first to let the service worker cache itself. Also: iOS sometimes blocks service workers in Private Browsing — make sure you weren't in Private when you first visited.

**Fonts look wrong** → The app uses Google Fonts (Fraunces / Inter Tight / JetBrains Mono). First load needs internet to fetch them, then they're cached for offline use.

**I lost an assessment** → Check the library — it's sorted by most-recently-edited. If it's genuinely gone, iOS may have cleared site data; this is why exporting completed assessments as PDF matters.

---

## Source attribution

SCAT6 content © Concussion in Sport Group / BMJ 2023 (Echemendia RJ, et al. Br J Sports Med 2023;57:622–631). The original publication permits free copying in current form for distribution to individuals, teams, groups and organisations. This digital implementation is for clinical use, not commercial redistribution.
