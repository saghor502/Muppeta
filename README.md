# Muppeta

This project has two parts:
- **AndroidWrapper** — an Android app that opens a webpage inside a phone/tablet
- **The Next.js web page** — the actual page the app will display (hosted on your computer)

Both your computer and your Android device must be on the **same Wi-Fi network** for this to work.

---

## What you need to install

### On your computer
- [Node.js](https://nodejs.org/) (LTS version)
- [Yarn](https://classic.yarnpkg.com/en/docs/install) — after installing Node.js, run `npm install -g yarn` in a terminal
- [Android Studio](https://developer.android.com/studio) — to build and install the Android app

### On your Android device
- Enable **Developer Options**: go to Settings → About Phone → tap "Build Number" 7 times
- Enable **USB Debugging**: Settings → Developer Options → USB Debugging → ON

---

## Step 1 — Find your computer's IP address

1. Open a terminal (Command Prompt or PowerShell on Windows)
2. Run: `ipconfig`
3. Look for **IPv4 Address** under your Wi-Fi adapter — it will look something like `192.168.1.176`
4. Write it down, you'll need it in the next steps

---

## Step 2 — Set up the Next.js page

The web page lives in the `anzu_landing_page` folder (or whichever Next.js project you're using).

### First time only — install dependencies

Open a terminal, navigate to the project folder and run:

```
yarn install
```

### Change the URL in the Android app

Open this file:

```
AndroidWrapper/app/src/main/java/com/example/realmomentsphotobooth/MainActivity.kt
```

Find this line near the bottom:

```kotlin
binding.webView.loadUrl("http://192.168.1.176:3000")
```

Replace `192.168.1.176` with **your own IP address** from Step 1.

Also open:

```
AndroidWrapper/app/src/main/res/xml/network_security_config.xml
```

And replace the IP address there too:

```xml
<domain includeSubdomains="true">192.168.1.176</domain>
```

### Start the page

In a terminal inside the Next.js project folder, run:

```
yarn dev
```

The page will now be running at `http://YOUR_IP:3000`. Leave this terminal open while using the app.

---

## Step 3 — Build and install the Android app

1. Open **Android Studio**
2. Click **Open** and select the `AndroidWrapper` folder
3. Wait for Gradle to sync (progress bar at the bottom — can take a few minutes the first time)
4. Plug your Android device into the computer via USB
5. Accept the "Allow USB Debugging" prompt on your phone if it appears
6. Press the green **Run** button (or press `Shift + F10`)
7. Select your device from the list and click OK

The app will install and open automatically on your device.

---

## Step 4 — Using the app

- Make sure the Next.js page is running (`yarn dev` from Step 2)
- Make sure your phone and computer are on the **same Wi-Fi network**
- Open the app — it will show a loading spinner while the page loads
- The back button navigates back within the page, just like a browser
- If the page doesn't load, double-check that the IP address in the app matches your computer's IP

---

## Changing the page URL

If the computer's IP address changes (this can happen when reconnecting to Wi-Fi), you need to:

1. Find the new IP with `ipconfig`
2. Update it in `MainActivity.kt` (the `loadUrl` line)
3. Update it in `network_security_config.xml`
4. Rebuild and reinstall the app from Android Studio

---

## Common issues

**"Webpage not available" / ERR_CLEARTEXT_NOT_PERMITTED**
The IP address in `network_security_config.xml` does not match the one in `MainActivity.kt`. Make sure both files have the same IP.

**Page loads on computer but not on phone**
Your phone and computer are probably on different networks. Make sure both are connected to the same Wi-Fi.

**App won't install**
Make sure USB Debugging is enabled and you accepted the prompt on the phone.

**Gradle sync fails in Android Studio**
Make sure you have an active internet connection the first time you open the project — it needs to download dependencies.
