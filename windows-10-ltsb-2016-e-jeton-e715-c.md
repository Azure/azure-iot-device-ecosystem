---
platform: windows 10 ltsb 2016
device: e-jeton-e715
language: c
---

Run a simple C sample on E-jeton-E715 device running Windows 10 LTSB 2016
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)


**About this document**

This document describes how to connect E-jeton-E715 device running Windows 10 LTSB 2016 with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-windows]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   E-jeton-E715 device.


<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Install the OS(Windows 10 LTSB 2016),make sure that your device cloud normal running.
-   Make sure that the device to be  network connected. 

<a name="Build"></a>
# Step 3: Build SDK and Run the sample

-   Start a new instance of Visual Studio 2015. Open the **iothub_client_sample_http.sln** solution in the **Desktop\azure-iot-sdk-c\iothub_client\samples\iothub_client_sample_http\windows** folder.

-   In Visual Studio, in **Solution Explorer**, navigate to **iothub_client_sample_http** project, open the *iothub_client_sample_htt.c** file.

-   Locate the following code in the file:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

-   In **Solution Explorer**, right-click the **iothub_client_sample_http** project, click **Debug**, and then click **Start new instance** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.


[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
