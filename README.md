# Training Companion — Setup & Maintenance Guide

## What This Is

A collection of personal training plan apps hosted as static HTML files on GitHub Pages. Each person gets their own standalone page with their full program, exercise videos, and progression logic — all running in the browser with zero backend, no logins, and no app store.

**Live URLs:**

| Person | URL |
|--------|-----|
| Landing page | https://rflsierra.github.io/training-companion/ |
| LP (wife) | https://rflsierra.github.io/training-companion/lp.html |
| RS (Rafael) | https://rflsierra.github.io/training-companion/rs.html |
| Javier (brother) | https://rflsierra.github.io/training-companion/javier.html |

**GitHub repo:** https://github.com/rflsierra/training-companion


---

## Repo Structure

```
training-companion/
├── index.html          ← Landing page with links to all plans
├── lp.html             ← LP's plan (postpartum strength + running)
├── rs.html             ← Rafael's plan (Coach81 v3)
├── javier.html         ← Javier's plan (powerlifting periodization)
├── icon_lp.png         ← Home screen icon — teal heart
├── icon_rs.png         ← Home screen icon — Coach81 dark blue
└── icon_javier.png     ← Home screen icon — dark with red arrows
```

Each HTML file is fully self-contained: React, styling, data, and logic are all in one file. No build step, no dependencies to install, no server to run. Open the file in any browser and it works.


---

## Adding to iPhone Home Screen

This makes it launch full-screen like a native app (no Safari toolbar).

1. Open the person's URL in **Safari** (this does not work from Chrome on iPhone)
2. Tap the **Share button** — the square with an upward arrow, at the bottom of the screen
3. Scroll down and tap **"Add to Home Screen"**
4. Edit the name if desired (it will suggest the name from the HTML meta tag — "Training", "Coach81", or "Powerlifting")
5. Tap **Add**

The icon will appear on the home screen. Tapping it opens the app full-screen.

**Notes:**
- The app loads from the internet each time, so an internet connection is needed (it's a tiny page, loads in under a second)
- If the URL is updated (you push changes to the repo), the home screen app automatically picks up the new version next time it's opened
- If the icon shows as a generic Safari compass, clear the home screen shortcut and re-add it — sometimes iOS caches the old version before the icon file was deployed


---

## Adding to Android Home Screen

1. Open the URL in **Chrome**
2. Tap the **three-dot menu** (top right)
3. Tap **"Add to Home screen"** or **"Install app"**
4. Confirm the name and tap **Add**

Same result — launches without browser chrome.


---

## How to Update a Plan

The files live only in the GitHub repo, not on your local machine (unless you clone it). There are three ways to update:

### Option A: Claude Code (recommended)

1. Generate the updated HTML file in a Claude chat (just ask for the changes)
2. Download the new file to any local folder
3. Open Claude Code in that folder and say:

```
Clone the training-companion repo from https://github.com/rflsierra/training-companion,
replace [filename].html with the updated version in this folder,
commit with message "Update [person]'s plan: [what changed]",
and push.
```

Claude Code handles the git operations. The live site updates automatically within 1–2 minutes after the push.

### Option B: Manual git

```bash
git clone https://github.com/rflsierra/training-companion.git
cd training-companion

# Copy the updated file in
cp /path/to/updated/file.html ./lp.html   # (or rs.html, javier.html)

git add .
git commit -m "Update LP plan: added week 13 progression"
git push
```

### Option C: GitHub web UI (quickest for small edits)

1. Go to https://github.com/rflsierra/training-companion
2. Click on the file you want to edit
3. Click the pencil icon (Edit)
4. Make changes directly in the browser
5. Commit

This works for small text changes (fixing a typo in an exercise name, changing a rep count). For larger changes, regenerate the whole file in Claude.


---

## How to Add a New Person

1. **Generate the HTML file** — ask Claude to build a training companion app for the person's program, following the same structure as the existing ones
2. **Generate an icon** — ask Claude to create a 180×180 PNG home screen icon
3. **Add to the repo** — drop both files in the repo and update `index.html` to add a link. Claude Code prompt:

```
In the training-companion repo:
1. Add [name].html and icon_[name].png
2. Update index.html to add a link — pick a label that fits the plan (e.g. "[Name]'s Training Plan", "Powerlifting Plan") → [name].html
3. Commit and push
```

4. **Share the URL** — send them `https://rflsierra.github.io/training-companion/[name].html` and tell them to add it to their home screen


---

## How the Apps Work (Technical Notes)

Each HTML file contains:

- **React 18** loaded from CDN (cdnjs.cloudflare.com)
- **Babel standalone** for JSX transpilation in the browser
- **All program data** hardcoded as JavaScript objects (days, exercises, sets, reps, phases, alternatives)
- **Custom CSS** — no Tailwind CDN, just inline styles and a `<style>` block
- **YouTube search links** for every exercise — tapping "Vid" opens a YouTube search for that exercise's form tutorial

There is no database, no user accounts, no saved state between sessions. The week selector and expanded/collapsed states reset when the page is refreshed. This is intentional — the app is a reference tool, not a logging app.

The `apple-touch-icon` meta tag in the `<head>` tells iOS which PNG to use for the home screen icon. The `apple-mobile-web-app-capable` meta tag enables full-screen mode when launched from the home screen.


---

## Troubleshooting

**"The site isn't loading"** — GitHub Pages can take 1–2 minutes to deploy after a push. Wait and refresh.

**"The home screen icon is a generic Safari icon"** — Remove the home screen shortcut, clear Safari cache (Settings → Safari → Clear History and Website Data), then re-add. This usually happens if the shortcut was created before the icon PNG was deployed.

**"Changes aren't showing up"** — Hard refresh in Safari: hold the refresh button and tap "Request Desktop Site" then back to mobile, or clear cache. The browser sometimes caches aggressively.

**"I want to test locally before pushing"** — Just open the HTML file directly in a browser (double-click it or `open file.html`). Everything works locally since there's no server-side logic.

**"Can I make it work offline?"** — Possible with a service worker, but not currently implemented. Would require adding a small JS file and a manifest. Ask Claude if you want this added.
