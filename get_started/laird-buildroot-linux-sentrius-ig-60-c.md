---
platform: laird buildroot linux
device: sentrius ig 60
language: c
---

Run a simple C sample on Sentrius IG60 running Laird Buildroot Linux
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

This document describes how to connect Sentrius IG60 device running Laird Buildroot Linux with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-windows]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Laird Sentrius IG60 device
-   Development PC with build environment

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Setup your Linux development environment acording to directions from the Azure IoT C SDK
  <https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux>

-   Clone the Azure IoT SDK

        git clone https://github.com/Azure/azure-iot-sdk-c.git

-   From the Laird Linux Releases page, download both the SDK and the prebuilt SD card image. <https://github.com/LairdCP/IG60-Laird-Linux-Release-Packages/releases>

-   Setup your Larid IG60 according to the driections: <https://documentation.lairdconnect.com/Builds/IG60-LINUX/latest/Content/Home.htm>

<a name="Build"></a>
# Step 3: Build SDK and Run the sample

-   From the prebilt IG 60 SDK, find the path to cmake toolchain file. Note this path, it will be used as a parameter with cmake.

        find . -name "toolchainfile.cmake"

-   Find the **iothub_ll_telemetry_sampe** example. It should be located in 
    ```
    /azure-iot-sdk-c/iothub_client/samples/iothub_ll_telemetry_sample/
    ```

-   Open the source file iothub_ll_telemetry_sample.c in a text editor.

-   Locate the following code in the file:

      static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

-   In the terminal, chage directories to `azure-iot-sdk-c/build_all/linux`

-   run `setup.sh`. This is only required once

-   Use the following command to build all device examples. Note that you will need to specify the proper path to the toolchain file found in a previous step.

        ./build.sh --toolchain-file <PATH to IG SDK>/share/buildroot/toolchainfile.cmake

-   The build output should be located in:

        /azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_ll_telemetry_sample

-   Use scp to copy the binary to the Sentrius IG60 and run

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer]
-   [Save IoT Hub messages to Azure data storage]
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub]
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]
-   [Remote monitoring and notifications with Logic Apps]

[manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[save iot hub messages to azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[use power bi to visualize real-time sensor data from azure iot hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[use azure web apps to visualize real-time sensor data from azure iot hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[weather forecast using the sensor data from your iot hub in azure machine learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[remote monitoring and notifications with logic apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md