<!--
This README describes the package. If you publish this package to pub.dev,
this README's contents appear on the landing page for your package.

For information about how to write a good package README, see the guide for
[writing package pages](https://dart.dev/tools/pub/writing-package-pages).

For general information about developing packages, see the Dart guide for
[creating packages](https://dart.dev/guides/libraries/create-packages)
and the Flutter guide for
[developing packages and plugins](https://flutter.dev/to/develop-packages).
-->


## Android setup

Add the following permissions to your manifest:

```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
```

Afterwards, include the following entries within the application tag, as follows:

```xml
<application
       ...
        <!-- configuration of background_locator_2 -->
        <receiver android:name="yukams.app.background_locator_2.BootBroadcastReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
            </intent-filter>
        </receiver>        
        <service android:name="yukams.app.background_locator_2.IsolateHolderService"
            android:permission="android.permission.FOREGROUND_SERVICE"
            android:exported="true"
            android:foregroundServiceType = "location"/>
    </application>
```

**NOTE** In Android 11 and above location permissions cannot be set automatically by the app (via the manifest file). Google writes on [this page](https://developer.android.com/training/location/permissions#request-background-location) that:

> On Android 11 (API level 30) and higher [...] the system dialog doesn’t include the **Allow all the time** option. Instead, users must enable background location on a settings page, as shown in figure 7.

Hence, for this plugin (and the example app) to work, you need to go to this settings screen and **manually** set the permission to "Alow all the time".

## iOS setup

Add the following entries to your `Info.plist` file

```xml
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>This app needs access to location when open and in the background.</string>
<key>NSLocationAlwaysUsageDescription</key>
<string>This app needs access to location when in the background.</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access to location when open.</string>
<key>UIBackgroundModes</key>
<array>
 <string>location</string>
</array>
```

Next, overwrite your `AppDelegate.swift` with:

```swift
import UIKit
import Flutter
import background_locator_2

func registerPlugins(registry: FlutterPluginRegistry) -> () {
    if (!registry.hasPlugin("BackgroundLocatorPlugin")) {
        GeneratedPluginRegistrant.register(with: registry)
    } 
}

@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
  ) -> Bool {
    GeneratedPluginRegistrant.register(with: self)
    BackgroundLocatorPlugin.setPluginRegistrantCallback(registerPlugins)
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }
}
```

## Features

Please file feature requests and bug reports

## Getting started

See the example app for a complete example of usage.

## Usage

```dart
  // configure the location manager
  LocationManager().interval = 1;
  LocationManager().distanceFilter = 0;
  LocationManager().notificationTitle = 'CARP Location Example';
  LocationManager().notificationMsg = 'CARP is tracking your location';

  // get the current location
  await LocationManager().getCurrentLocation();

  // start listen to location updates
  StreamSubscription<LocationDto> locationSubscription = LocationManager()
      .locationStream
      .listen((LocationDto loc) => print(loc));

  // cancel listening and stop the location manager
  locationSubscription?.cancel();
  LocationManager().stop();
```

See the example app for a complete example of usage.

```dart
const like = 'sample';
```

## Additional information

..
