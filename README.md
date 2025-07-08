# Guide: Hiding Root from Banking and Other Apps (2025)

This guide provides a step-by-step process for hiding root access from sensitive applications like banking apps (e.g., GCash) that use root detection and Google's Play Integrity API.

> **⚠️ Disclaimer: A Note on Root Solution, Compatibility, and Timeliness**
>
> The steps and modules in this guide are written specifically for **Magisk** and were successfully tested on a device running **Android 15**. The information is current as of **July 8, 2025**.
>
> While the core principles of hiding root may translate to other solutions like **KernelSU**, this guide has **not** been tested on those platforms. They have their own unique mechanisms and module ecosystems, so you will likely need to find alternative modules and adapt the steps accordingly.
>
> Furthermore, bypassing root detection is a constant "cat-and-mouse game." Success is never guaranteed and can depend on your specific device, software version, and security patch. Please be aware that these methods may become obsolete over time.

---

## 🛠️ Step 1: Initial Setup & Cleanup

Before installing new modules, it's crucial to start with a clean slate.

1.  **Update your Magisk App**: Ensure you are on the latest stable version of Magisk.
2.  **Hide the Magisk App**:
    *   Open Magisk -> Tap the **Settings** icon (⚙️) in the top right.
    *   Under "App", tap **Hide the Magisk app**.
    *   Give it a new name (e.g., "Settings" or "Manager") and let it reinstall.
3.  **Disable Conflicting Modules**:
    *   Open Magisk -> Go to the **Modules** tab.
    *   **Disable all existing modules** to avoid conflicts. You can re-enable non-essential ones later, one by one, if needed.
4.  **Clear App Data**:
    *   Go to your phone's Settings -> Apps.
    *   Find and clear storage/data for the following apps:
        *   The app you want to hide root from (e.g., `GCash`).
        *   `Google Play Services`
        *   `Google Play Store`

---

## 📦 Step 2: Install Required Modules & Apps

Download and install the following tools. You can find them on GitHub or XDA-Developers.

*   **Modules (Install via Magisk)**:
    *   [Play Integrity Fork](https://github.com/osm0sis/PlayIntegrityFork/releases)
    *   [Shamiko](https://github.com/LSPosed/LSPosed.github.io/releases)
    *   [Zygisk - LSPosed](https://github.com/JingMatrix/LSPosed/releases)
    *   [Zygisk Assistant](https://github.com/snake-4/Zygisk-Assistant/releases)
    *   [Zygisk Next](https://github.com/Dr-TSNG/ZygiskNext/releases)

*   **Apps (Install as normal APKs)**:
    *   [HideMyApplist](https://github.com/Dr-TSNG/Hide-My-Applist/releases)
    *   [Play Integrity API Checker](https://play.google.com/store/apps/details?id=gr.nikolasspyr.integritycheck)

After installing all Magisk modules, **reboot your device**.

---

## ⚙️ Step 3: Configure Magisk Settings

With the required modules installed, configure Magisk for optimal hiding.

1.  Open Magisk -> Tap the **Settings** icon (⚙️).
2.  **Disable Built-in Zygisk**: Ensure the `Zygisk` toggle is **OFF**. We are using Shamiko to manage this.
3.  **Disable DenyList Enforcement**: Ensure the `Enforce DenyList` toggle is **OFF**. Shamiko uses its own blocklist logic.
4.  **Configure DenyList**:
    *   Tap on `Configure DenyList`.
    *   Find your target app (e.g., `GCash`) and check the box next to it. Tap on the app name to expand it and **check all sub-processes**.
    *   Find `Google Play Services`. **Check the main box only**. Do *not* expand and check its sub-processes.
    *   Find `Google Play Store`. **Check the main box only**. Do *not* expand and check its sub-processes.
5.  **Verify Shamiko Mode**:
    *   It should report that Shamiko is in **Blacklist mode**.

---

## 🔒 Step 4: Configure LSPosed & HideMyApplist

This step prevents apps from detecting other root-related apps on your device.

1.  **Enable LSPosed**:
    *   You should see a notification from LSPosed after rebooting. If not, open the **LSPosed** app from your app drawer.
    *   Go to the **Modules** section.
    *   Enable **HideMyApplist** by turning on its toggle.
2.  **Configure HideMyApplist**:
    *   Open the **HideMyApplist** app.
    *   Go to **Template Manage**.
    *   Create a new **Blacklist Template**. Name it something memorable like `Hide Root Apps`.
    *   In this new template, add only the apps that have root access (Magisk, LSPosed, your file explorer, etc.) to the invisible apps.
    *   Also inside this new template, find your target app (e.g., `GCash`) and put it to the applied apps list.
    *   Go back to App manage and select your target app (e.g.,  `GCash`) and  `Enable hide`.

---

## ✅ Step 5: Verify Play Integrity

Now, let's check if the setup passes Google's integrity check.

1.  Open the **Play Integrity Fork** app.
2.  Press the **Check** button at the bottom.
3.  The app will run a check. You are looking for the result `MEETS_BASIC_INTEGRITY`.
4.  **If the check fails (shows X):**
    *   In the Play Integrity Fork app, tap **Action**.
    *   Delete Play Integrity API Check Storage and open the app.
    *   Press **Check** again.
    *   Repeat this "Delete Storage" and "Check" process a few times. It can sometimes take several attempts to get a passing result.
5.  Once you successfully see `MEETS_BASIC_INTEGRITY`, you are ready for the final step.

---

## 🚀 Step 6: Final Steps

1.  **Restart your device** one last time.
2.  After rebooting, open your target app (e.g., `GCash`). It should now launch without detecting root.

## ❓ Troubleshooting

*   **Still Detected?** Be sure that before doing this guide, you have cleared the storage for your targetted app, play store, and play services. Double-check that all conflicting Magisk modules are disabled, Shamiko is in blacklist mode, and you have correctly configured the DenyList and HideMyApplist.
*   **What about `MEETS_DEVICE_INTEGRITY`?** Passing `MEETS_DEVICE_INTEGRITY` is much harder and often requires a specific device fingerprint. For most apps, `MEETS_BASIC_INTEGRITY` is sufficient.
*   **SafetyNet vs. Play Integrity:** SafetyNet is deprecated. Play Integrity is the new standard for app-level security checks.

---

## ❤️ Acknowledgments

This guide is a compilation of community knowledge. Huge thanks to the developers of these essential tools:
*   **Magisk**: topjohnwu
*   **Play Integrity Fork**: osm0sis, chiteroman
*   **Shamiko**: LSPosed Team
*   **Zygisk - LSPosed**: JingMatrix
*   **HideMyApplist**: Dr-TSNG
*   **Zygisk Assistant**: snake-4, Yervant7, osm0sis
*   **Zygisk Next**: 5ec1cff, Dr-TSNG
