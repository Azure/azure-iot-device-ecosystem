---
platform: windows 10 iot enterprise
device: industrial pc
language: csharp
---

Run a simple Csharp sample on Industrial PC NYB Series device running Windows 10 IoT Enterprise
===
---

# Table of Contents
-   [Introduction](#introduction)
-   [Step 1: Prerequisites](#step-1-prerequisites)
-   [Step 2: Prepare your Device](#step-2-prepare-your-device)
-   [Step 3: Build and Run the Sample](#step-3-build-and-run-the-sample)
-   [Next steps](#next-steps)

# Introduction

**About this document**

This document describes how to connect Industrial PC NYB Series device running Windows 10 IoT Enterpris with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and Deploy Azure IoT SDK on device

# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment](https://catalog.azureiotsolutions.com/docs?title=Azure/azure-iot-sdk-c/doc/devbox_setup)
-   [Setup your IoT hub](https://catalog.azureiotsolutions.com/docs?title=Azure/azure-iot-device-ecosystem/setup_iothub)
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Industrial PC NYB Series device.

# Step 2: Prepare your Device

-   Plug power line and USB keyboard, mouse, monitor via DisplayPort, Ethernet to Internet as same as Desktop PCs.

# Step 3: Build and Run the Sample

-   Download the [Azure IoT SDK](https://github.com/Azure/azure-iot-sdk-csharp/releases/tag/v20151102) and the sample programs and save them to your local repository.
-   Start a new instance of Visual Studio 2015.
-   Open the **iothub\_csharp\_client.sln** solution in the <span style="color:red">`device`</span> folder in your local copy of the repository.
-   In Visual Studio, from Solution Explorer, navigate to the **samples** folder.
-   In the **DeviceClientAmqpSample** project, open the **Program.cs** file.
-   Locate the following code in the file:

```C#
private const string DeviceConnectionString = "<replace>";
```

-   Replace <span style="color:red">`<replace>`</span> with the connection string for your device.
-   In **Solution Explorer**, right-click the **DeviceClientAmqpSample** project, click **Debug**, and then click **Start new instance** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.
-   Use the **DeviceExplorer** utility to observe the messages IoT Hub receives from the **Device Client AMQP Sample** application.
-   Refer "Monitor device-to-cloud events" in [DeviceExplorer Usage document](https://catalog.azureiotsolutions.com/docs?title=Azure/azure-iot-sdk-csharp/tools/DeviceExplorer/doc/how_to_use_device_explorer) to see the data your device is sending.
-   Refer "Send cloud-to-device messages" in [DeviceExplorer Usage document](https://catalog.azureiotsolutions.com/docs?title=Azure/azure-iot-sdk-csharp/tools/DeviceExplorer/doc/how_to_use_device_explorer) for instructions on sending messages to device.

# Next steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-vscode-iot-toolkit-cloud-device-messaging)
-   [Save IoT Hub messages to Azure data storage](https://docs.microsoft.com/en-us/azure/iot-hub/tutorial-routing#routing-to-a-storage-account)
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
-   [Remote monitoring and notifications with Logic Apps](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)

[lnk-manage-iot-hub]: ../manage_iot_hub.md