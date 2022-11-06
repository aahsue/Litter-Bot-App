
Android BluetoothLeGatt Sample
===================================

This sample demonstrates how to use the Bluetooth LE Generic Attribute Profile (GATT)
to transmit arbitrary data between devices.

**This is the original Android BluetoothLeGatt sample but modified to run with Android 12+**

Original source repository: https://github.com/android/connectivity

Bluetooth LE Gatt example: https://github.com/android/connectivity-samples/tree/main/BluetoothLeGatt

As of Nov. 6th 2022 there is still an issue with the official Bluetooth LE Gatt example, that is caused 
due to change in permissions in newer Android SDK's. To solve the issue I changed the AndroidManifest.xml 
and DeviceScanActivity.java to get the example to work:

AndroidManifest.xml:
```plaintext
    <!-- Min/target SDK versions (<uses-sdk>) managed by build.gradle -->
    
    <!-- Declare this required feature if you want to make the app available to BLE-capable
    devices only.  If you want to make your app available to devices that don't support BLE,
    you should omit this in the manifest.  Instead, determine BLE capability by using
    PackageManager.hasSystemFeature(FEATURE_BLUETOOTH_LE) -->
    <uses-feature android:name="android.hardware.bluetooth_le" android:required="true"/>

    <!-- Min/target SDK versions (<uses-sdk>) managed by build.gradle -->
    <!-- Request legacy Bluetooth permissions on older devices. -->
    <uses-permission
        android:name="android.permission.BLUETOOTH"
        android:maxSdkVersion="30" />
    <uses-permission
        android:name="android.permission.BLUETOOTH_ADMIN"
        android:maxSdkVersion="30" />
    <!--
 Needed only if your app looks for Bluetooth devices.
         If your app doesn't use Bluetooth scan results to derive physical
         location information, you can strongly assert that your app
         doesn't derive physical location.
    -->
    <uses-permission
        android:name="android.permission.BLUETOOTH_SCAN"
        android:usesPermissionFlags="neverForLocation"
        tools:targetApi="s" />
    <!--
 Needed only if your app makes the device discoverable to Bluetooth
         devices.
    -->
    <uses-permission android:name="android.permission.BLUETOOTH_ADVERTISE" />
    <!--
 Needed only if your app communicates with already-paired Bluetooth
         devices.
    -->
    <uses-permission android:name="android.permission.BLUETOOTH_CONNECT" /> <!-- Needed only if your app uses Bluetooth scan results to derive physical location. -->
    <uses-permission
        android:name="android.permission.ACCESS_FINE_LOCATION"
        tools:ignore="CoarseFineLocation" />
```

DeviceScanActivity.java:
```plaintext
    /**
     * This block is for requesting permissions up to Android 12+
     *
     */

    private static final int PERMISSIONS_REQUEST_CODE = 191;
    private static final String[] BLE_PERMISSIONS = new String[]{
            Manifest.permission.ACCESS_COARSE_LOCATION,
            Manifest.permission.ACCESS_FINE_LOCATION,
    };
    @SuppressLint("InlinedApi")
    private static final String[] ANDROID_12_BLE_PERMISSIONS = new String[]{
            Manifest.permission.BLUETOOTH_SCAN,
            Manifest.permission.BLUETOOTH_CONNECT,
            Manifest.permission.BLUETOOTH_ADVERTISE,
            Manifest.permission.ACCESS_FINE_LOCATION
    };

    public static void requestBlePermissions(Activity activity, int requestCode) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S)
            ActivityCompat.requestPermissions(activity, ANDROID_12_BLE_PERMISSIONS, requestCode);
        else
            ActivityCompat.requestPermissions(activity, BLE_PERMISSIONS, requestCode);
    }
    
    in onCreate add:
    
    requestBlePermissions(this, PERMISSIONS_REQUEST_CODE);
```

see the issue: https://github.com/android/connectivity-samples/issues/268

Note: this code runs on actual Android devices (tested up to SDK 32 = Android 12) with Gradle 7.4.0.

Introduction
------------

This sample shows a list of available Bluetooth LE devices and provides
an interface to connect, display data and display GATT services and
characteristics supported by the devices.

It creates a [Service][1] for managing connection and data communication with a GATT server
hosted on a given Bluetooth LE device.

The Activities communicate with the Service, which in turn interacts with the [Bluetooth LE API][2].

[1]:http://developer.android.com/reference/android/app/Service.html
[2]:https://developer.android.com/reference/android/bluetooth/BluetoothGatt.html

Pre-requisites
--------------

- Android SDK 28
- Android Build Tools v28.0.3
- Android Support Repository

Screenshots
-------------

<img src="screenshots/1-main.png" height="400" alt="Screenshot"/> <img src="screenshots/2-detail.png" height="400" alt="Screenshot"/> 

Getting Started
---------------

This sample uses the Gradle build system. To build this project, use the
"gradlew build" command or use "Import Project" in Android Studio.

Support
-------

- Stack Overflow: http://stackoverflow.com/questions/tagged/android

If you've found an error in this sample, please file an issue:
https://github.com/android/connectivity

Patches are encouraged, and may be submitted by forking this project and
submitting a pull request through GitHub. Please see CONTRIBUTING.md for more details.
