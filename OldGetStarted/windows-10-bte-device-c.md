---
platform: windows 10
device: bte device
language: c
---

Run a simple C sample on BTE device running Windows 10
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"/>
# Introduction

**About this document**

This document describes how to connect BTE device running Windows 10 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-windows]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   BTE device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Follow the instructions from [device website](http://www.btenetworks.com)

<a name="Build"></a>
# Step 3: Build and Run the sample
## 3.1 Build SDK and sample
-   Please find Azure IoT SDKs for C sample code [here](https://github.com/Azure/azure-iot-sdk-c/).

### 3.1.1 Send Device Events to IOT Hub:
-   Start a new instance of Visual Studio 2017 and open project from here:

        iothub_client\samples\iothub_ll_telemetry_sample\windows\iothub_ll_telemetry_sample.sln

-   Go to `iothub_ll_telemetry_sample.c`, replace the **[device connection string]** placeholder with **connection string** of the device you have created in Provision your device and get its credentials and save the file.
-    An example of IoT Hub Connection String is as below:

     In the sample's main source file, find the line similar to this:

        static const char* connectionString = "[device connection string]";


     and replace [device connection string] with a valid device connection string for a device registered with your IoT Hub.

-   In Solution Explorer, right-click the **iothub\_ll\_telemetry\_sample** project, click Debug, and then click **Start new instance** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.

### 3.1.2 Receive messages from IoT Hub
-   Start a new instance of Visual Studio 2017 and open project from here:

-   Open `iothub_client\samples\iothub_ll_c2d_sample\windows\iothub_ll_c2d_sample.sln`

-   Go to **iothub\_ll\_c2d\_sample.c**, replace the `[device connection string]` placeholder with connection string of the device you have created in Provision your device and get its credentials and save the file. An example of IoT Hub Connection String is as below:

-   In the sample's main source file, find the line similar to this:

        <static const char* connectionString = "[device connection string]";

    and replace [device connection string] with a valid device connection string for a device registered with your IoT Hub.

-   In Solution Explorer, right-click the **iothub\_ll\_c2d\_sample** project, click Debug, and then click **Start new instance** to build and run the sample. 


## 3.2 Send Device Events to IoT Hub:

-   See [Manage IoT Hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md) to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md) to learn how to send cloud-to-device messages to the application.

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
[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
