---
platform: embedded yocto
device: grapeboard
language: c
---

Run a simple C sample on Grapeboard running Yocto
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

This document describes how to build Grapeboard running Yocto 2.0 with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Sign Up To Azure IoT Hub][sign-iot-hub]
-   Prepare Grapeboard
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Empty microSD card

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

Follow these steps to prepare the device.

## 2.1 Prepare build environment

Yocto BSP is verified to build using Debian Jessie Linux distribution. Install
it and follow the [instructions][yocto-bsp-doc] on further system configuration.

## 2.2 Fetch device BSP sources

-   Follow the Grapeboard Yocto BSP [documentation][yocto-bsp-doc] on how to fetch the sources.

-   Prepare default BSP configuration by sourcing the environment script:

        $ cd <bsp>
        $ source grapeboard-env

## 2.3 Build BSP

-   Build the Grapeboard BSP following the instructions in the [documentation][yocto-bsp-doc].

## 2.4 Deploy BSP

-   Format SD card into ext4.

-   Copy built BSP contents from the `<bsp>/build/tmp/deploy/images/grapeboard/scalys-base-image-grapeboard.tar.gz` to the ext4-formatted SD card.

## 2.5 Build Yocto SDK

-   Follow the [instructions][yocto-bsp-doc] to build Yocto SDK.

-   Install Yocto SDK on the development machine by executing generated install script

        $ <bsp>/build/tmp/deploy/sdk/poky-glibc-x86_64-grapeboard-image-base-aarch64-toolchain-2.5.sh

-   Copy the CMake toolchain file `<bsp>/build/tmp/sysroots-components/x86_64-nativesdk/nativesdk-cmake/opt/poky/2.5/sysroots/x86_64-pokysdk-linux/usr/share/cmake/OEToolchainConfig.cmake` into the Yocto SDK install location.

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Install SDK

-   Download Azure IoT Device SDK for C on the development machine:

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

## 3.2 Build SDK and sample

-   Add device connect string into below file by using `vi`:

        azure-iot-sdk-c/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample.c
        azure-iot-sdk-c/iothub_client/samples/iothub_ll_c2d_sample/iothub_ll_c2d_sample.c

-   Find the following place holder for device connection string:

        connectionString = "[device connection string]"

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Run following commands to build the SDK with the generated toolchain:

        cd azure-iot-sdk-c/build_all/linux
        ./build.sh  --toolchain-file <yocto-sdk>/OEToolchainConfig.cmake -cl --sysroot="<yoct-sdk>/sysroots/aarch64-poky-linux"

-   After a successful build, you can copy **azure-iot-sdk-c** folder onto the Grapeboard SD card with rootfs.

-   Insert SD card into Grapeboard and boot to the Linux prompt.

-   Navigate to samples directory:

        cd azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples

## 3.3 Send Device Events to IoT Hub:

-   Run the sample application using the following command:

        cd azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_ll_telemetry_sample
        ./iothub_ll_telemetry_sample

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.4 Receive messages from IoT Hub

-   Run the sample application using the following command:

        cd azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_ll_c2d_sample
        ./iothub_ll_c2d_sample

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

[Manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[Save IoT Hub messages to Azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[Use Power BI to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[Remote monitoring and notifications with Logic Apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[sign-iot-hub]: https://account.windowsazure.com/signup?offer=ms-azr-0044p
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[yocto-bsp-doc]: https://github.com/githaff/yocto-bsp/blob/master/readme.md
