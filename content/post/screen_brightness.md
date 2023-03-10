---
title: "Managing device screen brightness in your android application"
date: 2023-02-26T11:50:01+03:00
draft: false
---

Why might you need to control the brightness of the device screen? For example: You are developing an application that displays QR codes that are read by the scanner. For better readability, we need to maximize image contrast and for that we can change the brightness to the maximum.

Before start to control screen brightness, we must answer for one question: "Do we want to control the brightness of the screen only when our app is active, or does it need to record the setting value after our app is shut down?"

### Scenario 1: In-app screen brightness management

In this case, it will be enough to refer to the class `WindowManager.LayoutParams` and change one of its parameters. You can do this inside your Activity as follows:

{{< gist healthdatastore 0d180816a4ff778e77f7a4d7a39f344f >}}

Here, the parameter: `desiredScreenBrightness` is a number that varies from `0f` to `1.0f`.

Do we need to restore the previous screen brightness when leaving the Activity?

The answer is: "No". Since for each Activity we have a `Window` instance, which in turn has its own attributes `WindowManager.LayoutParams`, stored in the `mWindowAttributes` field.

### Scenario 2: App's state independent screen brightness management

To control the brightness of your device’s screen without being tied to your app’s lifecycle, you must meet the following conditions:

- you should add this permission to the manifest file:

``` xml
<uses-permission android:name="android.permission.WRITE_SETTINGS"/>
```

> ***NOTE:*** adding `tools:ignore="ProtectedPermissions"` helps to workaround android linter checks.

- If your app's target sdk version is 23 or higher `Settings.ACTION_MANAGE_WRITE_SETTINGS` intent must be sent at runtime. This permission is so-called "Special permission". The way to request this permission differs from standard runtime permission request. Each special permission has its own check method. For our case it's `Settings.System.canWrite()`.

### Conclusion

In most cases scenario 1 should be sufficient to your needs as an app developer unless you are need the app which can manipulate system screen brightness setting. And here Scenario 2 could be useful.
