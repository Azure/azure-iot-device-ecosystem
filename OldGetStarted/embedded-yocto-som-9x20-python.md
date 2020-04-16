---
platform: embedded yocto 2.0
device: som-9x20
language: python
---

Run a simple Python sample on SOM9x20 running Yocto 2.0
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

This document describes how to build SOM-9x20 and Starter board running Yocto 2.0 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Sign Up To Azure IoT Hub][sign-iot-hub]
-   [Prepare SOM-9x20][setup-hardware]
-   [Prepare your development environment][setup-SOM-9x20-yocto]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Empty microSD card 4GB at least or fastboot support host PC.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

You should have following steps to prepare the device.

## 2.1 Intall Yocto 2.0 into Linux Host PC

-   [Here][setup-yocto] is instruction how to install Yocto 2.0 and setup Linux Host PC.

## 2.2 Create projects and build images

-   [Here][setup-SOM-9x20-yocto] is instruction to create projects and build images.

## 2.3 Customize root file system

-   Use the syntax below and add features to your **conf/local.conf** file.

        MACHINE ??= "mdm9650"
        requrie conf/multilib.conf
        MULTILIBS = "multilib:lib32"
        DEFAULTTUNE_virtclass-multilib-lib32 = "armv7athf-neon"

        EXTRA_IMAGE_FEATURES = "debug-tweaks tools-debug tools-sdk dev-pkgs"
        IMAGE_INSTALL_append = " openssh sudo lib32-autoconf lib32-automake lib32-cpp lib32-cpp-symlinks lib32-gcc lib32-gcc-symlinks lib32-g++ lib32-g++-symlinks lib32-gettext lib32-make lib32-libstdc++ lib32-libstdc++-dev lib32-file lib32-coreutils lib32-libdaemon lib32-glibc lib32-glibc-dev lib32-python lib32-python-dev lib32-python-compiler lib32-python-numpy lib32-python-paho-mqtt lib32-cmake lib32-cmake-dev lib32-git lib32-curl lib32-curl-dev lib32-libcurl lib32-pkgconfig lib32-boost lib32-boost-dev lib32-valgrind lib32-valgrind-dev lib32-libcrypto lib32-libssl lib32-openssl lib32-openssl-conf lib32-openssl-dev lib32-uuid lib32-ossp-uuid-dev lib32-python-zlib lib32-util-linux lib32-zlib-dev ca-certificates"
        PACKAGECONFIG_pn-boost = " python"

-   Note that the space must be included before the first word in IMAGE\_INSTALL\_append and PACKAGECONFIG\_pn-boost section. Then build new image by following command.

        bitake via-image-gui

-   New image will be created in **tmp/deploy/images/apq8096-drone** directory.
-   Specifically, you can find an file like below:

        apq8096-sysfs.ext4

## 2.4 Flash system image by using fastboot

-   Connect your host PC and SOM-9x20 through USB OTG.
-   Power on SOM-9x20 into fastboot mode.
-   Using below command to flash system image.

        fastboot flash system apq8096-sysfs.ext4

## 2.5 boot the OS from UFS

-   Turn on the board and messages are shown through console over Debug Port.
-   Open and configure Putty or serial console terminal then check messages.

        msm LE.UM.2.3.2-01200-SDX24 apq8096 /dev/ttyHSL0

        apq8096 login:

-   You can login by **root** account
-   And password is **oelinux123**

## 2.6 Confirm software version

Check software version. **cmake** version is 3.3.1. **gcc** version is 4.9.3. And **python** is 2.7.9.

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

-   Navigate to samples folder by executing following command:

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

-   Run the sample application using the following command:

    **For AMQP protocol:**

        python iothub_client_sample.py -p amqp

    **For HTTP protocol:**

        python iothub_client_sample.py -p http

    **For MQTT protocol:**

        python iothub_client_sample.py -p mqtt

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
[setup-hardware]: http://cdn.viaembedded.com/products/docs/som-9x20/User_Manual/UM_SOM-9X20_v1.05_180510.pdf
[setup-SOM-9x20-yocto]: https://www.viatech.com/en/boards/modules/som-9x20/software-downloads/
[sign-iot-hub]: https://account.windowsazure.com/signup?offer=ms-azr-0044p
[setup-yocto]: https://www.viatech.com/en/boards/modules/som-9x20/software-downloads/
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md