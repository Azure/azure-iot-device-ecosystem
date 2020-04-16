---
platform: windows 10
device: lattepanda
language: c
---

Run a simple C sample on LattePanda device running Windows 10
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

This document describes how to connect [LattePanda](http://www.lattepanda.com) device running Windows 10 with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-windows]

    **Note:** LattePanda runs a full edition of Windows 10, so you are able to install the development environment on your LattePanda and run this example directly. You can also develop on another computer and then copy the released .exe file to your Lattepanda if you prefer. In this tutorial, we developed directly on a LattePanda.

-   [Setup your IoT hub][lnk-setup-iot-hub]

-   [Provision your device and get its credentials][lnk-manage-iot-hub]

-   [LattePanda](http://www.lattepanda.com)  device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Connect the power adapter, USB Keyborad/Mouse with [LattePanda](http://www.lattepanda.com).
-   Press the power button on the back of the device.
-   Wait until the operating system is ready.

<a name="Build"></a>
# Step 3: Build and Run the sample

-   Follow the instructions [here](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) to prepare your development environment. 

-   A folder **cmake_Win32** will be created under your user profile folder e.g. **c:\user\[yourusername]\cmake_Win32**. 

-   Start a new instance of Visual Studio 2015. Open the **azure_iot_sdks.sln** solution in the **cmake_Win32** folder.

-   In Visual Studio, in **Solution Explorer**, navigate to **simplesample_amqp** project, open the **simplesample_amqp.c** file.

-   Locate the following code in the file:

              static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Step-1:-Prerequisites) and save the changes.

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

-   In **Solution Explorer**, right-click the **simplesample_amqp** project, click **Debug**, and then click **Start new instance** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.

-   Use the **DeviceExplorer** utility to observe the messages IoT Hub receives from the application.

-   Refer "Monitor device-to-cloud events" in [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to see the data your device is sending.

-   Refer "Send cloud-to-device messages" in [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) for instructions on sending messages to device.

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
[lnk-setup-iot-hub]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md
[lnk-manage-iot-hub]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md

