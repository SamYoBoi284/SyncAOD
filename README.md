# Sync AOD

Native Android lifecycle experiment, using the Gradle and GitHub Actions pattern from [Sync-AI](https://github.com/SamYoBoi284/Sync-AI).

## Prototype
- Black clock screen with live time/date and double-tap dismissal.
- User-started foreground service observes screen-off and user-present broadcasts.
- Attempts to launch the AOD activity after screen-off; inspect Logcat tag `SyncAODLifecycle`.
- Dismissal remains active until the next user-present broadcast to reduce relaunch loops.

## Critical limitation
Android restricts starting activities from the background. Samsung One UI may reject or defer the screen-off launch, and a fully-off display cannot render this activity. This prototype tests that assumption instead of claiming to bypass it. It never dismisses or bypasses PIN/password/biometric security.

## Device test
1. Install the debug APK and open Sync AOD.
2. Grant notifications if prompted; start the experiment.
3. Lock the phone and observe whether the clock appears.
4. If blocked, collect Logcat entries filtered by `SyncAODLifecycle`.
5. If shown, double-tap to dismiss; unlock and lock again to test re-entry.
6. Stop the experiment from the app when finished.

Build with JDK 17, Gradle 8.10, Android Gradle Plugin 8.7.3, SDK 35 using `gradle assembleDebug`.
