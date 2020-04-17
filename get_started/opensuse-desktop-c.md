---
platform: opensuse
device: desktop
language: c
---

Run a simple C sample on device running openSUSE
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Step-1-Prerequisites)
-   [Step 2: Prepare your Device](#Step-2-PrepareDevice)
-   [Step 3: Build and Run the Sample](#Step-3-Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect devices running openSUSE with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Step-1-Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Computer with Git client installed and access to the
    [azure-iot-sdk-c](https://github.com/Azure/azure-iot-sdk-c) GitHub public repository.
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]


<a name="Step-2-PrepareDevice"></a>
# Step 2: Prepare your Device

-   Open a PuTTY session and connect to the device.

  > Note: You can skip this step if you just want to build the sample application without running it.

<a name="Step-3-Build"></a>
# Step 3: Build and Run the sample

<a name="setup"/>
## Setup the development environment

This section shows you how to set up a development environment for the Azure IoT device SDK for C on openSUSE.

1. Clone this repository ([azure-iot-sdk-c](https://github.com/Azure/azure-iot-sdk-c)) to the machine you are using.
2. Open a shell and navigate to the folder **c/build_all/linux** in your local copy of the repository.

3. Run the `setup_opensuse.sh` script to install the prerequisite packages and the dependent libraries.

4. Run the `build.sh` script.

This script builds the **iothub_client** and **serializer** libraries and their associated samples in a folder called "cmake" inside you home folder.

 > Note: you will not be able to run the samples until you configure them with a valid IoT Hub device connection string. For more information, see [Run sample on Linux](linux-desktop-c.md).

 <a name="buildrunapp"/>
## Run the sample

1. Open the file **c/serializer/samples/simplesample_amqp/simplesample_amqp.c** in a text editor.

2. Locate the following code in the file:
    ```
   static const char* connectionString = "[device connection string]";
    ```
3. Replace "[device connection string]" with the device connection string you noted [earlier](#Step-1-Prerequisites). Save the changes.

5. Save your changes and build the samples. To build your sample you can run the build.sh script in the **c/build_all/linux** directory.

6. Run the **azure-iot-sdk-c/cmake/iotsdk_linux/serializer/samples/simplesample_amqp/simplesample_amqp** sample application.

7.   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the **simplesample_amqp** application and how to send cloud-to-device messages to the **simplesample_amqp** application.

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
