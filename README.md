# Xiaomi / HyperOS App Lock Reset via ADB

A simple method to disable Xiaomi's App Lock without performing a factory reset, using Android Debug Bridge (ADB).

This method changes specific secure settings on the device to disable App Lock-related protection flags.

## Tested Devices

| Device | OS | Result |
|---|---|---|
| Xiaomi 11T | HyperOS (V816) | Working |

**Note:** This method has been confirmed on one Xiaomi 11T running HyperOS V816. It may work on other MIUI and HyperOS versions, but compatibility is not guaranteed.

## Requirements

- A Xiaomi device running MIUI or HyperOS
- A Windows, Linux, or macOS computer
- USB cable
- ADB (Android Platform Tools)
- USB debugging enabled
- USB debugging (Security Settings) enabled, if required by your ROM

## Step 1 — Enable USB Debugging

1. Open **Settings → About phone**.
2. Tap **OS version** several times to enable Developer Options.
3. Go to **Settings → Additional settings → Developer options**.
4. Enable **USB debugging**.
5. If available, enable **USB debugging (Security Settings)**.

Connect your phone to your computer and accept the USB debugging authorization prompt.

## Step 2 — Install ADB

Download Android Platform Tools:

https://developer.android.com/tools/releases/platform-tools

Extract the downloaded archive and open a terminal in the extracted folder.

Check your device connection:

```bash
adb devices
```

Your device should appear with the status `device`.

## Step 3 — Disable App Lock

Run these commands one by one:

```bash
adb shell settings put secure privacy_password_is_open 0
adb shell settings put secure access_control_lock_enabled 0
adb shell settings put secure app_lock_enabled 0
```

These are the settings that successfully disabled App Lock on our tested Xiaomi 11T.

## Step 4 — Verify the Changes

Run:

```bash
adb shell settings get secure privacy_password_is_open
adb shell settings get secure access_control_lock_enabled
adb shell settings get secure app_lock_enabled
```

Expected output:

```text
0
0
0
```

## Step 5 — Reboot Your Device

Restart your phone:

```bash
adb reboot
```

After the device restarts, check whether App Lock has been disabled.

## Troubleshooting

**Device shows as unauthorized**

- Check your phone for the USB debugging authorization prompt.
- Accept the connection.
- Try `adb devices` again.

**Permission denied**

Some ROM versions may restrict access to secure settings. USB debugging (Security Settings) may be required.

**App Lock is still enabled**

- Verify that all three settings return `0`.
- Restart your device.
- Your ROM may use a different security implementation.

## Important Warnings

- This method disables App Lock-related security settings. It does not recover or reveal your forgotten password.
- Disabling these settings may also affect other privacy-protection features.
- Do not use `pm clear` on Xiaomi Security Center.
- No factory reset is required.
- This method does not intentionally delete personal files or installed applications.
- Results may vary depending on the Xiaomi device, Android version, MIUI version, and HyperOS build.

## Credits

Created to help Xiaomi users who have forgotten their App Lock password and want to avoid a factory reset.

Tested and confirmed by **Sceasty** on a Xiaomi 11T running HyperOS V816.

If this method helped you, consider giving the repository a star!
