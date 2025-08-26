# 📱 Capacitor & React -- Feature Compatibility Guide

Capacitor lets you wrap a React (or any web) app into a native
Android/iOS app using a WebView.\
While most functionality works out-of-the-box, there are **important
limitations and differences** compared to running the app in a browser.

This document lists **what will work, what won't, and what needs special
handling**.

------------------------------------------------------------------------

## ✅ Works Without Issues

Anything that runs in a **mobile browser (Safari / Chrome on Android)**
generally works inside Capacitor. Examples:\
- Standard HTML, CSS, JavaScript, React components.\
- REST & GraphQL API calls (`fetch`, `axios`).\
- State management libraries (Redux, Zustand, Recoil, etc.).\
- Client-side routing (React Router, Next.js SPA mode).\
- LocalStorage, sessionStorage (with some size limits).\
- Cookies (with caveats for iOS; may need secure flags).\
- Authentication flows: - OAuth with redirects (Google, Facebook,
etc.).\
- JWT token authentication.\
- Basic media handling (images, audio, video via `<video>` and
`<audio>`).\
- Progressive enhancement (if not relying on service workers).

------------------------------------------------------------------------

## 🚫 Features That Do NOT Work

These are browser-only features that **do not work in WebView apps**:

1.  **Service Workers**
    -   No offline caching, background sync, or push via service
        workers.
2.  **Web Push Notifications**
    -   Standard browser push APIs (`Notification`, `PushManager`) don't
        work.\
    -   Must use native push systems (Firebase Cloud Messaging for
        Android, Apple Push Notification Service for iOS) via
        Capacitor's Push Notification plugin.
3.  **Background Sync & Background Fetch**
    -   Browser background sync APIs won't run when the app is closed or
        in the background.
4.  **Browser Extensions**
    -   No Chrome/Firefox extension APIs (`chrome.*` APIs).
5.  **PaymentRequest API**
    -   Browser-native payment UI (Google Pay, Apple Pay via web) may
        not work.\
    -   Solution: Use the payment provider's **native SDKs** or
        web-based hosted checkout.
6.  **Desktop-only APIs**
    -   Drag-and-drop file system APIs (`window.showOpenFilePicker`) not
        supported on mobile WebViews.

------------------------------------------------------------------------

## ⚠️ Features That Work with Limitations

Some features work but with differences compared to browsers:

### 1. Storage

-   **LocalStorage / sessionStorage** → works, but limited in size
    (\~5MB).\
-   **IndexedDB** → works, but can be unstable on iOS.\
-   **Workaround:** Use Capacitor's [Storage
    API](https://capacitorjs.com/docs/apis/storage) or [Filesystem
    API](https://capacitorjs.com/docs/apis/filesystem).

### 2. File Uploads & Downloads

-   `<input type="file">` → works, but UX is not always smooth
    (especially on iOS).\
-   Downloads via `<a download>` → may not work consistently.\
-   **Solution:** Use Capacitor's Filesystem, Camera, or Share plugins.

### 3. Background Execution

-   `setInterval` / `setTimeout` → paused when app goes to background.\
-   Background tasks (e.g., tracking location, syncing data) require
    **native plugins**.

### 4. WebRTC / Media APIs

-   Works inconsistently across Android/iOS WebViews.\
-   Accessing camera/mic via `getUserMedia()` may be blocked.\
-   **Solution:** Use Capacitor Camera/Media plugins or native SDKs
    (Agora, Twilio).

### 5. Cookies & Sessions

-   iOS WebView may clear cookies unexpectedly.\
-   Safari Intelligent Tracking Prevention (ITP) can block 3rd-party
    cookies.\
-   **Solution:** Use token-based authentication (JWT, OAuth with PKCE).

### 6. Navigation / Links

-   `window.open` → may not open external links properly.\
-   Use Capacitor's **Browser API** for in-app or system browser links.

### 7. Geolocation

-   Browser's `navigator.geolocation` may not be reliable.\
-   **Solution:** Use Capacitor's [Geolocation
    plugin](https://capacitorjs.com/docs/apis/geolocation).

------------------------------------------------------------------------

## 🛠 Native Features You Can Add with Capacitor Plugins

Capacitor bridges the gap by providing native APIs:\
- **Camera** (take pictures, select from gallery).\
- **Filesystem** (read/write files).\
- **Push Notifications** (Firebase, APNs).\
- **Geolocation** (GPS tracking).\
- **Device Info** (battery, network status, hardware details).\
- **Clipboard, Share, Haptics, Splash Screen, App Launcher**, etc.

------------------------------------------------------------------------

## ⚡ Performance Considerations

-   **Animations / heavy UI**: WebView may feel slower than native UI.\
-   **Large data rendering**: React tables with thousands of rows may
    lag.\
-   **3D rendering (WebGL, Three.js)**: Works but drains battery and can
    stutter.\
-   For **games, AR, VR, or highly interactive apps** → consider React
    Native or Flutter.

------------------------------------------------------------------------

## 📌 Summary

-   Capacitor = ✅ Best choice if you want to **reuse your web app** and
    add **basic native features**.\
-   🚫 Avoid using **service workers, push notifications, background
    sync, or browser-only APIs**.\
-   ⚠️ Use **Capacitor plugins** for reliable storage, camera,
    geolocation, and push notifications.\
-   ⚡ If your app needs **heavy performance / gaming / AR** → go native
    (React Native / Flutter).
