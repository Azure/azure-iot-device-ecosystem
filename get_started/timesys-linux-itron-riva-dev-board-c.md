---
platform: timesys linux 
device:  itron riva dev edge board
language: c
---

Run a simple C sample on Itron Riva Dev Edge Board device running TimeSys Linux
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Setup the development environment](#SetupEnvironment)
-   [Step 4: Build and Run the Sample](#BuildAndRun)
-   [Tips](#Tips)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect Itron Riva Dev Edge Board device running TimeSys Linux with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Itron Riva Dev Edge Board device.
-   [Download curl library](https://curl.haxx.se/download.html)

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   [Setup Itron Riva Dev Edge Board SDK](https://www.itron.com/partners/developer/resources/getting-started-software)
-   [Connect Itron Riva Dev Edge Board to your host computer](https://www.itron.com/partners/developer/resources/edge-getting-started)

<a name="SetupEnvironment"></a>
# Step 3: Setup the development environment

This section shows you how to set up a development environment for the Azure IoT device SDK for C on Itron Riva Dev Edge Board.

### Install dependencies under /usr/lib

-   Download curl-7.35.0.tar.gz for cross compiling.
-   Download the Microsoft Azure IoT Device SDK by issuing following command:

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Navigate to the folder c/build_all/linux in your local copy of the repository.

<a name="BuildAndRun"></a>
# Step 4: Build and Run the sample

## 4.1 Setting up cmake to cross compile

-   Switch to the SDK directory tree.

        cd ~/azure-iot-sdk-c/build_all/linux

-   Using the text editor of your choice, create a new file in this directory and call it toolchain-itronriva.cmake and add following lines to it:

        INCLUDE(CMakeForceCompiler)
        
        SET(CMAKE_SYSTEM_NAME Linux)    # this one is important
        SET(CMAKE_SYSTEM_VERSION 1)     # this one not so much
        
        # this is the location of the amd64 toolchain targeting the Itron Riva Edge
        SET(CMAKE_C_COMPILER $ENV{RIVA_ROOT}/bin/armv7l-timesys-linux-uclibcgnueabi-gcc)
        
        # this is the file system root of the target
        SET(CMAKE_FIND_ROOT_PATH $ENV{RIVA_ROOT})
        
        # search for programs in the build host directories
        SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
        
        # for libraries and headers in the target directories
        SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
        SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

-   Save the toolchain file. Your cross compilation environment is now complete.

## 4.2 Build the sample

-   Edit the following file using any text editor of your choice: azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Build the SDK using following command.

        ./build.sh --toolchain-file toolchain-itronriva.cmake -cl --sysroot=$YOUR_SYS_ROOT -cl -L~/toolchain/lib/openssl/engines -cl -lssl -cl -lcrypto

## 4.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

<a name="Tips"></a>
# Tips

-   If you just want to build the serializer samples, run the following commands:

    ```
    cd ./c/serializer/build/linux
    make -f makefile.linux all
    ```

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
