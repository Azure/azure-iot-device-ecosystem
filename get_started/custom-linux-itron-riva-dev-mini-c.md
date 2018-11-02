---
platform: custom linux
device: itron riva dev mini
language: c
---

Run a simple C sample on Itron Riva Dev Mini device running Custom Linux
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Setup Itron SDK environment](#environment)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect Itron Riva Dev Mini device running Custom Linux with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Itron Riva Dev Mini device.

<a name="environment"></a>
# Step 2: Setup Itron SDK environment

-   Request itron-isom-development-environment.sh from [admin@itronriva.com](mailto:admin@itronriva.com).

-   Follow [Software Development on Itron Riva Dev board](https://www.itron.com/na/partners/developer/resources/getting-started-software) SDK guide.

### Step 2.1: Confirm software version

-   Check software version: cmake version is 3.5.1, gcc version is 5.4.0, and git 2.7.4

        cmake --version
        gcc --version
        git --version

### Step 2.2: Prepare Itron Isom SDK for azure-iot-sdk-c development

-   Run the following commands in your development environment where you installed `itron-isom-development-environment.sh`
  
        wget http://curl.haxx.se/download/curl-7.37.0.tar.gz
        curl-7.37.0.tar.gz
        sudo cp -R curl-7.37.0/include/curl ~/timesys/itron-isom/rfs/usr/include/
        rm -R curl-7.37.0
        rm curl-7.37.0.tar.gz
        sudo rm ~/timesys/itron-isom/rfs/usr/lib/libssl.so.1.0.0
        sudo rm ~timesys/itron-isom/rfs/usr/lib/libcrypto.so.1.0.0
        sudo ln -s /home/itronee/timesys/itron-isom/rfs/usr/lib/libcurl.so.4.3.0~/timesys/itron-isom/toolchain/lib/libcurl.so

<a name="PrepareDevice"></a>
# Step 3: Prepare your Device

To purchase an Itron Riva Dev Mini, please visit Itron Riva developer site.
Device starting guide can be found here.

### Step 3.1: Copylibuuid.so.1.3.0over to your Itron Riva Dev Mini device

-   Copy `~/timesys/itron-isom/toolchain/lib/libuuid.so.1.3.0` over to your Itron Riva Dev Mini device’s /usr/lib directory via a flash drive or serial port transfer
-   Create `symlinks to libuuid.so.1.3.0. and ca-bundle.crt` 

        ln -s /usr/lib/libuuid.so.1.3.0 /usr/lib/libuuid.so.1
        ln -s /usr/lib/libuuid.so.1.3.0 /usr/lib/libuuid.so
        ln -s /usr/share/ncat/ca-bundle.crt /etc/ssl/certs/653b494a.0

<a name="Build"></a>
# Step 4: Build and Run the sample

### Step 4.1: Build SDK and sample

-   Download the Microsoft Azure IoT Device SDK for C to the board by issuing the following command on the board:

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Navigate to ~/azure-iot-sdk-c/build_all/linux/ and create toolchain-riva.cmake

        cd ~/azure-iot-sdk-c/build_all/linux/
        nanotoolchain-riva.cmake

-   Use below toolchain-riva.cmakeexample and edit paths to point to your Itron Riva toolchain environment

        nano~/azure-iot-sdk-c/build_all/linux/toolchain-riva.cmake

        INCLUDE(CMakeForceCompiler)

        SET(CMAKE_SYSTEM_NAME Linux)
        SET(CMAKE_SYSTEM_VERSION 8)

        # this is the location of the amd64 toolchain targeting the Riva Edge
        SET(CMAKE_C_COMPILER /home/jonathan/test/itron-isom/toolchain/bin/armv7l-timesys-linux-uclibcgnueabi-gcc)

        # this is the file system root of the target
        SET(CMAKE_FIND_ROOT_PATH /home/itronee/timesys/itron-isom/toolchain/)
        SET(OPENSSL_INCLUDE_DIR /home/itronee/timesys/itron-isom/toolchain/include)
        SET(CURL_LIBRARY /home/itronee/timesys/itron-isom/rfs/usr/lib/libcurl.so)
        SET(CURL_INCLUDE_DIR /home/itronee/timesys/itron-isom/rfs/usr/include/)
        SET(CURL_LIBRARIES /home/itronee/timesys/itron-isom/rfs/usr/lib)

        # search for programs in the build host directories
        SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)

        # for libraries and headers in the target directories
        SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
        SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

-   Edit sample with your IoT hub device connection string obtained in Step 1 and uncomment desired application protocol

        cd~/azure-iot-sdk-c/iothub_client/samples/iothub_ll_telemetry_sample/
        cp iothub_ll_telemetry_sample.ciothub_ll_telemetry_sample.c.original
        nanoiothub_ll_telemetry_sample.c

-   Build azure-iot-sdk-c and sample and have --sysroot point to your toolchain directory

        cd ~/azure-iot-sdk-c/build_all/linux/
        ./build.sh --toolchain-file toolchain-riva.cmake -cl --sysroot=/home/itronee/timesys/itron-isom/toolchain/

### Step 4.2: Send Device Events to IoT Hub:

-   Copy built `~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample` over to your Itron Riva Dev Mini device’s /root directoryvia a flash drive or serial port transfer
-   Run sample `iothub_ll_telemetry_sample` to send data to your IoT hub

        cd /root
        ./iothub_ll_telemetry_sample

-   See [Manage IoT Hub](https://github.com/neeraj-khanna/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md) to learn how to send cloud-to-device messages to the application.

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
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md