# SCENE — Android app (your own, no watermark)

This is a complete Android Studio project that wraps your live site
**https://scene.aarchiinfotech.in/public/** into an app you fully own — no
watermark, no third-party service, no monthly fee. It:

- Opens your site full-screen with your SCENE icon + navy splash.
- **Grants the camera** to the web page so the marshal QR scanner works.
- Handles **file upload** (photo on the registration form) and **ticket download**.
- Opens `tel:` / `mailto:` / WhatsApp links natively; keeps your site in-app.
- Hardware back button navigates web history.

You only ever rebuild the APK to change the icon, name, or permissions — any
content change on your website shows up in the app automatically.

---

## What you need (one-time)

1. **Android Studio** (free): https://developer.android.com/studio
   Install it with the default SDK. First launch downloads the SDK (~a few GB).

That's it. The project uses the Gradle wrapper, which Android Studio sets up for you.

---

## Build the APK — step by step

1. **Unzip** `SceneApp.zip` somewhere (e.g. Desktop).
2. Open **Android Studio** → **Open** → select the `SceneApp` folder → OK.
3. Wait for "Gradle sync" to finish (bottom status bar). First time it downloads
   Gradle + the Android libraries — needs internet, takes a few minutes.
   - If it asks to upgrade the Android Gradle Plugin, click **"Don't upgrade"** /
     keep as is — the project is already set for a current stable version.
4. Build the APK: top menu **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
5. When it finishes, a notification says "APK(s) generated". Click **locate** to
   find it, or look in:
   ```
   SceneApp/app/build/outputs/apk/debug/app-debug.apk
   ```
6. Copy `app-debug.apk` to an Android phone and install it (allow
   "Install unknown apps" if prompted). Done.

The **debug** APK is perfect for sharing with your team and side-loading. For the
Play Store you need a **signed release** build — see below.

---

## Test the camera (important)

1. Open the app → log in as a **marshal** → open an event → pick a sub-event →
   the scanner page.
2. Android asks for **camera permission** → tap **Allow**.
3. The camera preview appears and scanning works (green ✓ / red ✗ popups).

If the preview is black: make sure you deployed the updated `scanner.php` to your
server (it adds the WebView camera fallback), and that you tapped Allow.

---

## Change the app name / package / URL

- **App name:** `app/src/main/res/values/strings.xml` → `app_name`.
- **Website URL:** `MainActivity.java` → `START_URL` (already your site).
- **Package id (applicationId):** `app/build.gradle` → `applicationId`
  (currently `in.aarchiinfotech.scene`). Keep it unique and stable — changing it
  after publishing makes it a different app.
- **Icon:** replace the `ic_launcher*.png` files under `app/src/main/res/mipmap-*`
  (already generated from your SCENE logo). Or in Android Studio: right-click
  `res` → New → Image Asset.

---

## Make a signed RELEASE APK/AAB (for Google Play or a smaller app)

1. Android Studio → **Build → Generate Signed Bundle / APK**.
2. Choose **APK** (for side-loading) or **Android App Bundle** (for Play).
3. **Create new keystore** → pick a safe location + passwords → **remember these;
   you need the same keystore for every future update.**
4. Select **release**, finish. The signed file lands in
   `app/build/outputs/`.
5. For Play: upload the **.aab**, add your Privacy Policy URL
   (`https://scene.aarchiinfotech.in/public/privacy`), and when asked about camera
   use, answer: *"QR code scanning for event attendance."*

---

## Files in this project (what each does)

- `MainActivity.java` — the WebView, camera-permission bridge, downloads, file
  upload, link handling.
- `AndroidManifest.xml` — permissions (Internet, Camera), portrait, launcher.
- `app/build.gradle` — build config (SDK levels, applicationId, dependencies).
- `res/values/strings.xml` — app name.
- `res/values/themes.xml` + `values-night/` — status bar + splash colors.
- `res/drawable/splash.xml` — brand splash while the site loads.
- `res/mipmap-*` — your app icons (normal + round + adaptive).
- `ic_launcher-web-512.png` — 512×512 icon for the Play Store listing.

---

## Troubleshooting

- **Gradle sync fails / "SDK not found":** let Android Studio install the SDK it
  offers; then **File → Sync Project with Gradle Files**.
- **"Camera permission" loop:** uninstall + reinstall so Android re-prompts.
- **App shows old pages:** it loads your live site — just deploy updated files to
  the server; no rebuild needed.
- **Build plugin version error:** if your Android Studio is older, it may ask to
  change the Android Gradle Plugin version — accept its recommended version; the
  code is standard and compiles on current toolchains.
