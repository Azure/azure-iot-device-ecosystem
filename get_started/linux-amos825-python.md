---
platform: embedded yocto 1.7
device: amos825
language: python
---

Run a simple Python sample on AMOS825 running Yocto 1.7
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

This document describes how to build AMOS825 and Starter board running Yocto 1.7 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Sign Up To Azure IoT Hub][sign-iot-hub]
-   [Prepare AMOS825][setup-hardware]
-   [Prepare your development environment][setup-AMOS825-yocto]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Empty microSD card 4GB at least.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

You should have following steps to prepare the device.

## 2.1 Intall Yocto1.7 into Linux Host PC

-   [Here][setup-yocto] is instruction how to install Yocto 1.7 and setup Linux Host PC.

## 2.2 Create projects and build images

-   [Here][setup-AMOS825-yocto] is instruction to create projects and build images.

## 2.3 Customize root file system

-   Use the syntax below and add features to your **conf/local.conf** file.

        EXTRA_IMAGE_FEATURES = "debug-tweaks tools-debug tools-sdk dev-pkgs"
        IMAGE_INSTALL_append = " strace cmake boost"
        PACKAGECONFIG_pn_boost = " python"

-   Note that the space must be included before the first word in IMAGE_INSTALL_append and PACKAGECONFIG_pn_boost section. Then build new image by following command.

        bitake via-image-gui

-   New image will be created in **tmp/deploy/images/imx6qvab820** directory.
-   Specifically, you can find an file like below:

        via-image-gui-imx6qvab820-{date}.rootfs.sdcard


## 2.4 Create microSD card

-   umount your 4GB size SD card.
-   Using below command to flash image into SD card.

        sudo dd if=via-image-gui-imx6qvab820-{date}.rootfs.sdcard of=/dev/sdx bs=1M conv=fsync


## 2.5 boot the OS from microSD card

-   Turn on the board and messages are shown through console over Debug Port.
-   Open and configure Putty or serial console terminal then check messages.

        Poky (Yocto Project Reference Distro) 1.7 imx6qvab820 /dev/ttymxc1

        imx6qvab820 login:

-   You can login by **root** account


## 2.6 Confirm software version

Check software version. **cmake** version is 2.8.1. **gcc** version is 4.9.1. And **python** is 2.7.3.

        cmake --version
        gcc --version
        python --version


<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Install SDK

-   Download Azure IoT Device SDK for Python by following commands.

        git clone --recursive https://github.com/Azure/azure-iot-sdk-python.git

## 3.1 Build SDK and sample

-   Run following commands to build the SDK:

        cd azure-iot-sdk-python/build_all/linux
        ./build.sh    

-   After a successful build, the `iothub_client.so` Python extension module is copied to the **python/device/samples** folder.

- Navigate to samples folder by executing following command:

        cd azure-iot-sdk-python/device/samples/

-   Edit the following file using `vi`text editor:

        vi iothub_client_sample.py

-   Find the following place holder for device connection string:

        connectionString = "[device connection string]"

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

## 3.2 Send Device Events to IoT Hub:

-   Run the sample application using the following command:

    **For AMQP protocol:**

        python iothub_client_sample.py -p amqp

    **For HTTP protocol:**

        python iothub_client_sample.py -p http

    **For MQTT protocol:**

        python iothub_client_sample.py -p mqtt

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

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
[setup-hardware]: https://cdn.viaembedded.com/products/docs/amos-825/user_manual/UM_AMOS-825_v1.04_170417.pdf
[setup-AMOS825-yocto]: https://cdn.viaembedded.com/products/docs/amos-825/Linux_quick_start_guide/AMOS-825_Linux_BSP_v3.0.2_Quick_Start_Guide_v1.00_20170112.pdf
[sign-iot-hub]: https://account.windowsazure.com/signup?offer=ms-azr-0044p
[setup-yocto]: https://cdn.viaembedded.com/products/docs/amos-825/Linux_quick_start_guide/AMOS-825_Linux_BSP_v3.0.2_Quick_Start_Guide_v1.00_20170112.pdf
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md