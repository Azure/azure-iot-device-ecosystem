---
platform: linux
device: IoT Gateway UM-120
language: c
---

Run a simple C sample on UM-120 device running linux
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Setup the development environment][setup-devbox-linux]
-   [Step 4: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect **UM-120** device running **Linux** with Azure IoT SDK. This process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:
-   Computer with a Git client installed so that you can access the azure-iot-sdk-c code on GitHub.
-   UM-120 Device.
-   Ubuntu x86 machine (for cross compiling) 
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-  Connect to the UM-120 using the RS-232C for sample execute, and data transfer.

<a name="Setup"></a>
# Step 3: Setup the development environment

This section shows you how to set up a development environment for the Azure IoT device SDK for C on UM-120.

## Install dependencies under /opt/toolchain

-   Download msdk-4.4.7-mips-EB-3.10-0.9.33-m32t-131227b.tar.gz  for cross compiling.

-  Download the Microsoft Azure IoT Device SDK by issuing following command:

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Navigate to the folder **c/build_all/linux** in your local copy of the repository.

<a name="Build"></a>
# Step 4: Build and Run the sample

## 4.1 Setting up cmake to cross compile

-   Switch to the SDK directory tree.

    ```
    cd ~/azure-iot-sdk-c/build_all/linux
    ```

-   Using the text editor of your choice, create a new file in this directory and call it toolchain-um120.cmake and add following lines to it:

    ```
	SET(CMAKE_CROSSCOMPILING TRUE)
	SET(CMAKE_SYSTEM_PROCESSOR mips)
	SET(CMAKE_SYSTEM_NAME Linux)
	SET(CMAKE_SYSTEM_VERSION 1)
	SET(CMAKE_C_COMPILER /opt/toolchain/msdk-4.4.7-mips-EB-3.10-0.9.33-m32t-131227b/bin/mips-linux-gcc)
	SET(CMAKE_CXX_COMPILER /opt/toolchain/msdk-4.4.7-mips-EB-3.10-0.9.33-m32t-131227b/bin/mips-linux-g++)
	SET(CMAKE_FIND_ROOT_PATH /opt/toolchain/msdk-4.4.7-mips-EB-3.10-0.9.33-m32t-131227b)
	SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER) 
	SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY) 
	SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
    ```

-   Save the toolchain file. Your cross compilation environment is now complete.

## 4.2 Build the sample

-   Edit the following file using any text editor of your choice:
        azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Build the SDK using following command.

    ```
    sudo ./azure-iot-sdk-c/build_all/linux/build.sh --toolchain-file azure-iot-sdk-c/build_all/linux/toolchain-um120.cmake --run-unittests
    ```

## 4.3 Deploy the sample

-   Transfer the sample executable iothub_client_sample_mqtt to target board directory /tmp.

-   Before run the sample ,the device must clock calibrating ,use following command to calibrate.

        ntp_inet -h [NtpServer]

## 4.4 Send Device Events to IoT Hub:

        /tmp/iothub_client_sample_mqtt

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 4.5 Receive messages from IoT Hub

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
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

