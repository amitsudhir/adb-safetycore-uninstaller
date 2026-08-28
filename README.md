# Android SafetyCore Uninstaller

A simple guide for removing the Google SafetyCore package from an Android device using **ADB (Android Debug Bridge)** without root access.

> **Package:** `com.google.android.safetycore`

## ⚠️ Disclaimer

This guide modifies the installed package state for the current Android user. Use it only on devices you own or are authorized to modify.

Removing system or system-integrated packages may affect device functionality. Results can vary depending on the Android version, manufacturer, and device configuration.

This guide does **not** provide root access and does not permanently delete the underlying system files.

## Requirements

- Windows PC
- Android device
- USB cable
- Internet connection
- ADB Platform Tools
- USB Debugging enabled

## 1. Download ADB Platform Tools

Open **Command Prompt (CMD)** and run:

```cmd
powershell -Command "Invoke-WebRequest -Uri 'https://dl.google.com/android/repository/platform-tools-latest-windows.zip' -OutFile 'C:\platform-tools.zip'; Expand-Archive -Path 'C:\platform-tools.zip' -DestinationPath 'C:\' -Force; Remove-Item 'C:\platform-tools.zip'"
```

This downloads the official Google Platform Tools and extracts them to:

```text
C:\platform-tools
```

## 2. Enable USB Debugging

On the Android device:

**Settings → About Phone → tap Build Number 7 times**

Then open:

**Settings → Developer Options → USB Debugging**

Enable **USB Debugging**.

## 3. Connect the Device

Connect the Android device to the PC using a USB cable.

If the phone asks:

> Allow USB debugging?

Tap **Allow**.

## 4. Verify ADB Connection

Open CMD and run:

```cmd
cd C:\platform-tools
adb devices
```

If the device appears as:

```text
XXXXXXXX    unauthorized
```

unlock the phone and approve the USB debugging prompt.

Run again:

```cmd
adb devices
```

A successful connection should look like:

```text
XXXXXXXX    device
```

## 5. Remove Google SafetyCore

Run:

```cmd
adb shell pm uninstall -k --user 0 com.google.android.safetycore
```

If the command returns:

```text
Success
```

the package has been uninstalled for the current user.

## 6. Reboot the Device

Run:

```cmd
adb reboot
```

The device will restart.

## Restore the Package

If you need to restore the package for the current user, try:

```cmd
adb shell cmd package install-existing com.google.android.safetycore
```

If the package is available as a system package on the device, this should restore it for the current user.

## Troubleshooting

### `adb is not recognized`

Make sure you are inside the Platform Tools directory:

```cmd
cd C:\platform-tools
```

Then run:

```cmd
adb devices
```

### Device shows `unauthorized`

Unlock the Android device and accept the **USB debugging authorization** prompt.

### `Failure` during uninstall

The package may be protected or restricted on that particular device or Android version. Do not assume the same command will work on every device.

## How It Works

The command:

```cmd
adb shell pm uninstall -k --user 0 com.google.android.safetycore
```

uses Android's Package Manager (`pm`) through ADB.

- `adb shell` — executes a command on the Android device
- `pm` — Android Package Manager
- `uninstall` — requests package removal
- `-k` — keeps the package's data/cache
- `--user 0` — applies the operation to Android's primary user
- `com.google.android.safetycore` — target package

This is a **non-root** method.

## Important

Do not blindly uninstall other system packages using commands found online. Some packages are required for Android or manufacturer-specific functionality and removing them can cause instability.

Use ADB package-management commands only on devices you own or have permission to modify.

## License

This project is provided for educational and informational purposes.