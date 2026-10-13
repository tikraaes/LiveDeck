# LiveDeck

A phone app for TikTok LIVE screen sharing. Four modes:

- **Slides** – your photos and text slides. Tap right/left or swipe to change. Optional auto-advance.
- **Overlay** – headline, sub line, LIVE badge, countdown timer, goal bar and a scrolling ticker on a colour, green screen or your own photo.
- **Board** – a whiteboard to draw and write by hand.
- **Script** – a scrolling teleprompter with speed, text size and mirror.

Tap **Present** to hide every control so only the content is on screen. Exit with the faint × at the top of the screen or the Back button.

Everything you add is saved on the phone (photos in the app's own storage).

---

## Install on Android (APK)

You need a free GitHub account. No computer software required.

1. Create a new repository on github.com (for example `livedeck`).
2. Upload **everything in this folder**, including the hidden `.github` folder, to the `main` branch.
   - Easiest from a computer: drag the folder contents onto the repository page.
3. Open the **Actions** tab. The workflow **Build Android APK** starts by itself (about 5–8 minutes).
4. When it shows a green tick, open the run and download **LiveDeck-APK** under *Artifacts*. It is a zip with `app-debug.apk` inside.
5. On the phone, open the APK and allow "Install unknown apps" for your browser or Files app when asked.

Every time you change a file and push to `main`, a new APK is built.

## Or install as a web app (no APK)

1. In the repository: **Settings → Pages → Source: GitHub Actions**.
2. **Actions → Publish web app (GitHub Pages) → Run workflow**.
3. Open the Pages link in Chrome on the phone, tap ⋮ → **Install app** / **Add to Home screen**.

## Going live

1. Prepare your content in LiveDeck.
2. TikTok → **+** → **LIVE** → choose the screen-sharing option (often called **Mobile gaming**) → **Go LIVE**.
3. Switch to LiveDeck and tap **Present**.

TikTok LIVE needs an 18+ account and usually 1,000 followers. Screen sharing isn't on every account; TikTok LIVE Studio on a computer is the alternative.

---

## Changing the app

All of the app is in **`www/index.html`** (styles, screens and code in one file).

- Colours and fonts: the `:root { … }` block at the top of the `<style>`.
- Default slides, overlay text and sample script: `DEFAULTS` in the `<script>`.
- **Add a new mode** (for example a poll or a QR code screen):
  1. add `<div class="view" data-view="myid">…</div>` inside `#stage` (what viewers see),
  2. add `<div class="ctl" data-ctl="myid">…</div>` inside `#panel` (your controls),
  3. add `{ id: 'myid', label: 'My mode', icon: '<svg…>', enter(){}, leave(){} }` to `MODES`.
  The tab appears automatically.
- After editing, bump `VERSION` in `www/sw.js` so the web-app version refreshes.

### Project files

| Path | What it is |
|---|---|
| `www/index.html` | The whole app |
| `www/manifest.webmanifest`, `www/sw.js`, `www/icons/` | Web-app install and offline support |
| `capacitor.config.json`, `package.json` | Wraps the web app as an Android app (Capacitor 6) |
| `android-extra/MainActivity.java` | Keeps the screen on and hides the status bar in the APK |
| `assets/` | App icon source for the APK |
| `.github/workflows/build-apk.yml` | Builds the APK on GitHub |
| `.github/workflows/deploy-web.yml` | Publishes the web-app version on GitHub Pages |

### Building on your own computer (optional)

Needs Node 20, JDK 17 and Android Studio.

```
npm install
npx cap add android
cp android-extra/MainActivity.java android/app/src/main/java/com/livedeck/app/MainActivity.java
npx cap sync android
npx cap open android      # then Build > Build APK
```
