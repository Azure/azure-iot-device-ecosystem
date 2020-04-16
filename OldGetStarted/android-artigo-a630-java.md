---
platform: android
device: artigo-a630
language: java
---

Run a simple JAVA sample on ARTiGO-A630 running Android
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect ARTiGO-A630 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Connect the device to internet.
-   Download and install latest JDK from [here](<http://www.oracle.com/technetwork/java/javase/downloads/index.html>).
-   Download [Android Studio](<https://developer.android.com/studio/index.html>) on your Windows machine and follow the installation instructions.
-   Computer with Git client installed and access to the [azure-iot-sdk-java](https://github.com/Azure/azure-iot-sdk-java) GitHub public repository.
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Make sure desktop is ready as per instructions given in [Prepare your development environment](#Setup_DevEnv).
-   Plug in your device to your development machine with a USB cable. If you're developing on Windows, you might need to install the appropriate USB driver for your device. For help installing drivers, see the [OEM USB Drivers](<https://developer.android.com/studio/run/oem-usb.html>) document.
-   Connect the device to internet.
-   On host PC, run adb shell and execute following commands:
-   ./adb devices
- 	./adb shell
-  	Turn on Wifi and configure to connect to internet (commands to configure wifi to connect to internet automatically...)

<a name="Build"></a>
# Step 3: Build and Run the sample

## 3.1 Modify and Build the Samples

1.  Start a new instance of Android Studio and open Android project from [here](android-sample-code):

        azure-iot-sdk-java/device/samples/android-sample/

2.  Go to MainActivity.java, replace the [device connection string] placeholder with connection string of the device you have created in Provision your device and get its credentials and save the file. An example of IoT Hub Connection String is as below:

        HostName=[YourIoTHubName];SharedAccessKeyName=[YourAccessKeyName];SharedAccessKey=[YourAccessKey]

3. Build your project by going to ***Build menu > Make Project***.

## 3.2 Run and Validate the Samples

  In this section you will run the Azure IoT client SDK samples to validate communication between your device and Azure IoT Hub. You will send messages to the Azure IoT Hub service and validate that IoT Hub has successfully receive the data. You will also monitor any messages sent from the Azure IoT Hub to client.

### 3.2.1 Run the Sample:

-   Select one of your project's files and click Run from the toolbar.
-   In the Choose Device window that appears, select the Choose a running device radio button, select your device, and click OK.
-   Android Studio will install the app on your connected device and starts it.
 
### 3.2.2 Send Device Events to IoT Hub

- 	See [Manage IoT Hub]([lnk-manage-iot-hub]) to learn how to observe the messages IoT Hub receives from the application.
- 	As soon as you run the app on your device (or emulator), it will start sending messages to IoTHub.
- 	Check the Android Monitor window in Android Studio. Verify that the confirmation messages show an OK. If not, then you may have incorrectly copied the device hub connection information.

### 3.2.3 Receive messages from IoT Hub

- 	See [Manage IoT Hub]([lnk-manage-iot-hub]) to learn how to send cloud-to-device messages to the application.
- Click the Receive Messages button from the sample App UI loaded on your device or in the emulator. If you modify the application code to receive message right after sending message, you could skip this step.
- 	Check the Android Monitor window in Android Studio. You should be able to see the command received.

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer]
-   [Save IoT Hub messages to Azure data storage]
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub]
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]
-   [Remote monitoring and notifications with Logic Apps]   

[Manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[Save IoT Hub messages to Azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[Use Power BI to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[Remote monitoring and notifications with Logic Apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[android-sample-code]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples/android-sample
[mainactivity-source-code]: https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-samples/android-sample/app/src/main/java/com/iothub/azure/microsoft/com/androidsample/MainActivity.java
