---
edit_link: https://github.com/apache/cordova-plugin-device/blob/master/README.md
title: Device
plugin_name: cordova-plugin-device
plugin_version: master
description: Get device information.
---

<!-- WARNING: This file is generated. See fetch_docs.js. -->

<!--
# license: Licensed to the Apache Software Foundation (ASF) under one
#         or more contributor license agreements.  See the NOTICE file
#         distributed with this work for additional information
#         regarding copyright ownership.  The ASF licenses this file
#         to you under the Apache License, Version 2.0 (the
#         "License"); you may not use this file except in compliance
#         with the License.  You may obtain a copy of the License at
#
#           http://www.apache.org/licenses/LICENSE-2.0
#
#         Unless required by applicable law or agreed to in writing,
#         software distributed under the License is distributed on an
#         "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
#         KIND, either express or implied.  See the License for the
#         specific language governing permissions and limitations
#         under the License.
-->

# cordova-plugin-device

[![Android Testsuite](https://github.com/apache/cordova-plugin-device/actions/workflows/android.yml/badge.svg)](https://github.com/apache/cordova-plugin-device/actions/workflows/android.yml) [![Chrome Testsuite](https://github.com/apache/cordova-plugin-device/actions/workflows/chrome.yml/badge.svg)](https://github.com/apache/cordova-plugin-device/actions/workflows/chrome.yml) [![iOS Testsuite](https://github.com/apache/cordova-plugin-device/actions/workflows/ios.yml/badge.svg)](https://github.com/apache/cordova-plugin-device/actions/workflows/ios.yml) [![Lint Test](https://github.com/apache/cordova-plugin-device/actions/workflows/lint.yml/badge.svg)](https://github.com/apache/cordova-plugin-device/actions/workflows/lint.yml)

This plugin defines a global `device` object, which describes the device's hardware and software.
Although the object is in the global scope, it is not available until after the `deviceready` event.

```js
document.addEventListener("deviceready", onDeviceReady, false);
function onDeviceReady() {
    console.log(device.cordova);
}
```

## Installation

    cordova plugin add cordova-plugin-device

## Properties

- device.cordova
- device.model
- device.platform
- device.uuid
- device.version
- device.manufacturer
- device.isVirtual
- device.serial
- device.sdkVersion (Android only)

## device.cordova

Returns the Cordova platform's version that is bundled in the application.

The version information comes from the `cordova.js` file.

This property does not display other installed platforms' version information. Only the respective running platform's version is displayed.

Example:

If Cordova Android 10.1.1 is installed on the Cordova project, the `cordova.js` file, in the Android application, will contain `10.1.1`.

The `device.cordova` property will display `10.1.1`.

### Supported Platforms

- Android
- Browser
- iOS

## device.model

The `device.model` returns the name of the device's model or
product. The value is set by the device manufacturer and may be
different across versions of the same product.

### Supported Platforms

- Android
- Browser
- iOS

### Quick Example

```js
// Android: Pixel 4             returns "Pixel 4"
//          Motorola Moto G3    returns "MotoG3"
// Browser: Google Chrome       returns "Chrome"
//          Safari              returns "Safari"
// iOS:     iPad Mini           returns "iPad2,5"
//          iPhone 5            returns "iPhone5,1"
// See https://www.theiphonewiki.com/wiki/Models
// OS X:                        returns "x86_64"
//
var model = device.model;
```

### Android Quirks

- Gets the [model name](https://developer.android.com/reference/android/os/Build.html#MODEL).

### iOS Quirks

The model value is based on the identifier that Apple supplies.

If you need the exact device name, e.g. iPhone 13 Pro Max, a mapper needs to be created to convert the known identifiers to the associated device name.

Example: The identifier `iPhone14,3` is associated to the device `iPhone 13 Pro Max`.

For the full list of all identifiers to device names, see [here](https://www.theiphonewiki.com/wiki/Models)

## device.platform

Get the device's operating system name.

```js
var string = device.platform;
```
### Supported Platforms

- Android
- Browser
- iOS

### Quick Example

```js
// Depending on the device, a few examples are:
//   - "Android"
//   - "browser"
//   - "iOS"
//
var devicePlatform = device.platform;
```

## device.uuid

Get the device's Universally Unique Identifier ([UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)).

```js
var string = device.uuid;
```

### Description

The details of how a UUID is generated are determined by the device manufacturer and are specific to the device's platform or model.

### Supported Platforms

- Android
- iOS

### Quick Example

```js
// Android: Returns a random 64-bit integer (as a string, again!)
//
// iOS: (Paraphrased from the UIDevice Class documentation)
//         Returns the [UIDevice identifierForVendor] UUID which is unique and the same for all apps installed by the same vendor. However the UUID can be different if the user deletes all apps from the vendor and then reinstalls it.
//
var deviceID = device.uuid;
```

### Android Quirk

The `uuid` on Android is a 64-bit integer (expressed as a hexadecimal string). The behaviour of this `uuid` is different on two different OS versions-

**For < Android 8.0 (API level 26)**

In versions of the platform lower than Android 8.0, the `uuid` is randomly generated when the user first sets up the device and should remain constant for the lifetime of the user's device.

**For Android 8.0 or higher**

The above behaviour was changed in Android 8.0. Read it in detail [here](https://developer.android.com/about/versions/oreo/android-8.0-changes#privacy-all).

On Android 8.0 and higher versions, the `uuid` will be unique to each combination of app-signing key, user, and device. The value is scoped by signing key and user. The value may change if a factory reset is performed on the device or if an APK signing key changes.

Read more here https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID.

### iOS Quirk

The `uuid` on iOS uses the identifierForVendor property. It is unique to the device across the same vendor, but will be different for different vendors and will change if all apps from the vendor are deleted and then reinstalled.
Refer [here](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor) for details.
The UUID will be the same if app is restored from a backup or iCloud as it is saved in preferences. Users using older versions of this plugin will still receive the same previous UUID generated by another means as it will be retrieved from preferences.

### OS X Quirk

The `uuid` on OS X is generated automatically if it does not exist yet and is stored in the `standardUserDefaults` in the `CDVUUID` property.

## device.version

Get the operating system version.

    var string = device.version;

### Supported Platforms

- Android
- Browser
- iOS

### Quick Example

```js
// Android:    Froyo OS would return "2.2"
//             Eclair OS would return "2.1", "2.0.1", or "2.0"
//             Version can also return update level "2.1-update1"
//
// Browser:    Returns version number for the browser
//
// iOS:     iOS 3.2 returns "3.2"
//
var deviceVersion = device.version;
```

## device.manufacturer

Get the device's manufacturer.

    var string = device.manufacturer;

### Supported Platforms

- Android
- iOS

### Quick Example

```js
// Android:    Motorola XT1032 would return "motorola"
// iOS:     returns "Apple"
//
var deviceManufacturer = device.manufacturer;
```

## device.isVirtual

whether the device is running on a simulator.

```js
var isSim = device.isVirtual;
```

## device.sdkVersion (Android only)

Get the Android device's SDK version ([SDK_INT](https://developer.android.com/reference/android/os/Build.VERSION#SDK_INT)).

### Supported Platforms

- Android

### OS X and Browser Quirk

The `isVirtual` property on OS X and Browser always returns false.

## device.serial

Get the device hardware serial number ([SERIAL](https://developer.android.com/reference/android/os/Build.html#SERIAL)).

```js
var string = device.serial;
```

### Supported Platforms

- Android
- OS X

### Android Quirk

As of Android 9, the underlying native API that powered the `uuid` property is deprecated and will always return `UNKNOWN` without proper permissions. Cordova have never implemented handling the required permissions. As of Android 10, **all** non-resettable device identifiers are no longer readable by normal applications and will always return `UNKNOWN`. More information can be [read here](https://developer.android.com/about/versions/10/privacy/changes#non-resettable-device-ids).

## device.isiOSAppOnMac

The iOS app is running on the Mac desktop (Apple Silicon ARM64 processor, M1 or newer). 
This parameter is only returned for iOS V14.0 or later, and is not returned for Android devices.

```js
var boolean = device.isiOSAppOnMac;
```

### Supported Platforms

- iOS

---

## iOS Privacy Manifest

As of May 1, 2024, Apple requires a privacy manifest file to be created for apps and third-party SDKs. The purpose of the privacy manifest file is to explain the data being collected and the reasons for the required APIs it uses. Starting with `cordova-ios@7.1.0`, APIs are available for configuring the privacy manifest file from `config.xml`.

This plugin comes pre-bundled with a `PrivacyInfo.xcprivacy` file that contains the list of APIs it uses and the reasons for using them.

However, as an app developer, it will be your responsibility to identify additional information explaining what your app does with that data.

In this case, you will need to review the "[Describing data use in privacy manifests](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_data_use_in_privacy_manifests)" to understand the list of known `NSPrivacyCollectedDataTypes` and `NSPrivacyCollectedDataTypePurposes`.

For example, if you collected the device ID for app functionality and analytics, you would write the following in `config.xml`:

```xml
<platform name="ios">
    <privacy-manifest>
        <key>NSPrivacyTracking</key>
        <false/>
        <key>NSPrivacyTrackingDomains</key>
        <array/>
        <key>NSPrivacyAccessedAPITypes</key>
        <array/>
        <key>NSPrivacyCollectedDataTypes</key>
        <array>
            <dict>
                <key>NSPrivacyCollectedDataType</key>
                <string>NSPrivacyCollectedDataTypeDeviceID</string>
                <key>NSPrivacyCollectedDataTypeLinked</key>
                <false/>
                <key>NSPrivacyCollectedDataTypeTracking</key>
                <false/>
                <key>NSPrivacyCollectedDataTypePurposes</key>
                <array>
                    <string>NSPrivacyCollectedDataTypePurposeAnalytics</string>
                    <string>NSPrivacyCollectedDataTypePurposeAppFunctionality</string>
                </array>
            </dict>
        </array>
    </privacy-manifest>
</platform>
```

Also, ensure all four keys—`NSPrivacyTracking`, `NSPrivacyTrackingDomains`, `NSPrivacyAccessedAPITypes`, and `NSPrivacyCollectedDataTypes`—are defined, even if you are not making an addition to the other items. Apple requires all to be defined.
