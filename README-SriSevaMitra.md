# Sri Seva Mitra (com.srisevamitra.assistant)

Android helper for TTD seva booking pages in Chrome: a Lakshmi-icon floating button, an
accessibility engine that runs the booking flow, autofill for plain text boxes, and a Quick
Settings tile. Replaces the older "TTD Booking Helper" build and keeps all of its options.

## Get the APK file (no Android Studio needed)
This environment has no Android SDK, so the APK has to be compiled once on your side. Easiest way:
1. Create a free GitHub account and a new **private** repository.
2. Upload everything in this folder (drag and drop the files on the repo page, keep `.github/workflows`).
3. Open the repo -> **Actions** -> **Build APK** -> **Run workflow**.
4. When it finishes (about 5 minutes), open the run and download **SriSevaMitra-apk**. Unzip it to get `SriSevaMitra.apk`, copy it to your phone and install it.

Alternative with Android Studio (Koala or newer): open the folder, let it sync, then Build -> Build APK(s).
Debug-signed APKs install fine on any phone (allow "install unknown apps" for your file manager).

## What is in the app
| Feature | Where |
|---|---|
| Lakshmi Devi icon (stylised vector art, app icon + bubble) | launcher, floating button |
| Floating button, draggable, with panel | Start full booking / Fill form only / Fill remaining IDs only / Scan form (15 s) / Stop / Open app / Close |
| Quick Settings tile "Seva Fill" (one-tap ON/OFF for the floating button) | main screen -> Add Quick Settings tile |
| Autofill service for text boxes (email, city, state, country, pincode, name, age, ID number) | main screen -> Set as autofill service |
| 8-state booking engine (calendar, slot, persons, Continue, pilgrim form, Continue, review, final click) | "Start automation" or panel |
| App package field (empty = any app) | main screen |
| Permission checklist and live status | top of main screen |
| Scan report copy (to check page layout) | main screen -> Copy last scan report |
| Auto-enable Accessibility via WRITE_SECURE_SETTINGS | one-time adb grant, below |
| Volume-key shortcut | Settings -> Accessibility -> Sri Seva Mitra -> Shortcut |

## First-time setup on the phone
1. Open the app. Allow notifications.
2. **1. Allow display over other apps** -> switch ON.
3. **2. Open Accessibility settings** -> Sri Seva Mitra -> ON. If greyed out ("Restricted setting"): Settings -> Apps -> Sri Seva Mitra -> three dots -> **Allow restricted settings**, then retry.
4. Optional, skips step 3 in future: on a PC run
   `adb shell pm grant com.srisevamitra.assistant android.permission.WRITE_SECURE_SETTINGS`
   then tap **Auto-enable Accessibility**.
5. Fill and **Save settings**, tap **Start floating button**. Open the TTD page in Chrome and tap the Lakshmi bubble.

## Test order
1. Open the TTD booking form and tap **Scan form (15 s)**. Copy the report from the app and keep it; it shows exactly how Chrome exposes the fields.
2. **Fill form only** on the pilgrim page, check what filled.
3. **Start full booking** with "Auto-press final Confirm" OFF. It stops at the review page.
4. Logs: `adb logcat -s TTDBooking`.

## Safety behaviour
- Payment keywords (payment gateway, net banking, cards, Razorpay, BillDesk, PayU) stop the run instantly.
- CAPTCHA and OTP are never touched.
- Every failure posts a notification naming the state and reason. Time budget is 300 s plus wait seconds.
- Use it only for your own bookings and respect TTD's terms of use.

## Not carried over from the old APK
- The old app's unlimited "+ Add pilgrim" saved list (this version keeps 2 pilgrims, matching TTD's 2-person limit).
- Field matching was written without seeing TTD's live page, so run the scan once and adjust `Patterns` in `StateMachine.kt` if a field is missed.

Build settings: compileSdk/targetSdk 34, minSdk 26, Kotlin 1.9.24, AGP 8.5.2, Gradle 8.7, JDK 17. Dependencies: androidx.core-ktx and appcompat only.
