# Build the SCENE APK free in the cloud (no Android Studio)

This project includes a GitHub Actions workflow that compiles the APK for you on
GitHub's free servers. You push the project once, GitHub builds it, you download
the `.apk`. Takes ~3–5 minutes.

You need: a free **GitHub account** (github.com). Nothing installed on your PC.

---

## Option A — Upload via the GitHub website (easiest, no git)

1. Go to https://github.com/new and create a repository, e.g. `scene-app`
   (Private is fine). Click **Create repository**.
2. On the next page click **“uploading an existing file”**.
3. **Unzip `SceneApp.zip` on your computer**, then drag **everything inside the
   `SceneApp` folder** (the `app` folder, `.github` folder, `build.gradle`,
   `settings.gradle`, etc.) into the upload box.
   - Important: upload the *contents* of `SceneApp`, so that `build.gradle` and the
     `.github` folder sit at the **top level** of the repo (not inside a `SceneApp/`
     subfolder).
   - Make sure the hidden **`.github`** folder is included. If your file manager
     hides it, enable “show hidden files”, or use Option B below.
4. Click **Commit changes**.
5. Go to the **Actions** tab → you’ll see **“Build Android APK”** running
   (or click it → **Run workflow** if it didn’t auto-start).
6. When the green ✓ appears, click the run → scroll to **Artifacts** →
   download **`SceneApp-debug-apk`**. Unzip it → that’s your **`app-debug.apk`**.
7. Copy the APK to an Android phone and install (allow “install unknown apps”).

---

## Option B — With git (if you prefer the command line)

```bash
cd SceneApp
git init
git add -A           # includes the hidden .github folder
git commit -m "SCENE app"
git branch -M main
git remote add origin https://github.com/<you>/scene-app.git
git push -u origin main
```
Then open the repo’s **Actions** tab and download the APK artifact as in step 6.

---

## Test after install

1. Open the app → marshal login → event → sub-event → scanner.
2. Tap **Allow** on the camera prompt → the preview appears and scanning works.
   (Make sure the updated `scanner.php` is deployed on your server first.)

---

## Notes

- This produces a **debug** APK — perfect for installing and sharing with your
  team. It is not a Play Store build.
- For **Google Play** you need a *signed release* build (AAB). Tell me and I’ll add
  a release workflow that signs it with a keystore you create (the signing secrets
  go into GitHub → Settings → Secrets, never in the code).
- Any change to your website shows in the app automatically — you only rebuild the
  APK to change the icon, name, or permissions.

## If the build goes red
Open the failed run → click the **Build debug APK** step to see the error, paste it
to me, and I’ll fix it. The most common cause is the `.github` folder not being
uploaded (Option A, step 3) — then no build starts at all.
