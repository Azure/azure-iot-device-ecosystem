---
platform: fedora
device: artik-1020
language: c
---

Run a simple C sample on Samsung ARTIK-1020 device running Fedora 22
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Tips](#tips)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect Samsung ARTIK-1020 device running Fedora 22 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Computer with Git client installed and access to the
    [azure-iot-sdk-c](https://github.com/Azure/azure-iot-sdk-c) GitHub
    public repository.
-   Samsung ARTIK-1020 device to certify.
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
After unboxing your Samsung ARTIK-1020 devices, please follow our Getting Started documentations at <https://developer.artik.io/documentation/getting-started-beta/> to power up your board, and set up the serial communication between your ARTIK-1020 device and your host computer.

ARTIK-1020 supports both Ethernet and WiFi. If you want to use WiFi on your ARTIK-1020, please refer to <https://developer.artik.io/documentation/developer-guide/configuring-wifi-on-artik-10.html> to configure WiFi on your board.

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

-   Open a PuTTY session and connect to the device.

-   Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by issuing the following commands from the command line on your board:

        dnf check-update -y

        dnf install libcurl-devel openssl-devel gcc-c++ make cmake git

-   Download the Microsoft Azure IoT Device SDK for C to the board by issuing the following command on the board::

        git clone https://github.com/Azure/azure-iot-sdk-c.git

-   Edit the following file using any text editor of your choice:
  
    **For AMQP protocol:**

        azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp.c

    **For HTTPS protocol:**

        azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Step-1:-Prerequisites) and save the changes.

-   Build the SDK samples using the following command:

        ./azure-iot-sdk-c/build_all/linux/build.sh

## 3.2 Send Device Events to IoT Hub

-   Run the sample by issuing following command:

    **For AMQP protocol:**

        azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp

    **For HTTPS protocol:**

        azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

<a name="tips"></a>
# Tips

- If you just want to build the serializer samples, run the following commands:

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
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

