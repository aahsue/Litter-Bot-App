
Litter Bot App
===================================

This Android app acts as a remote control for the Litter Bot. It uses Bluetooth LE Generic Attribute Profile (GATT) to write data from the app to the GATT server on the ESP-32.

This app is built off of the following:

Original source repository: https://github.com/android/connectivity

Bluetooth LE Gatt example: https://github.com/android/connectivity-samples/tree/main/BluetoothLeGatt

Modified Bluetooth LE Gatt example for Android 12+: https://github.com/AndroidCrypto/BluetoothLeGattSdk33


Introduction
------------

As of now, this app shows a list of available Bluetooth LE devices and allows you to connect to the Litter Bot and view GATT services and characteristics, as well as write to the Litter Bot GATT server to control the Litter Bot.


Pre-requisites
--------------

- Android SDK 28
- Android Build Tools v28.0.3
- Android Support Repository