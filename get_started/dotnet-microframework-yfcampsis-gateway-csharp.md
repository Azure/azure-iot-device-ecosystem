---
platform: dotnet microframework
device: iot.net field gateway
language: csharp
---

Run a simple Csharp sample on YFSoft Campsis Gateway device running .Net Microframework
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

This document describes how to connect YFSoft Campsis Gateway device running .Net Microframework with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Computer with Git client installed and access to the
    [azure-iot-sdk-csharp](https://github.com/Azure/azure-iot-sdk-csharp) GitHub public repository.
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Innovactive YFSoft Campsis Gateway device.

#### Install Visual Studio 2015 and Tools

-   To create Windows .NET Micro Framework solutions, you will need to install [Visual Studio 2015](https://www.visualstudio.com/en-us/products/vs-2015-product-editions.aspx). You can install any edition of Visual Studio, including the free Community edition.

    After installing Visual Studio, goto Tools option in the menu bar and click on *Extensions and Updates*. Search for 'netmf' and install .NET Micro Framework SDK.

-   Depending on specific hardware revision of Innovactive YFSoft Campsis Gateway, YFSoft SDK could be needed in order to build and run provided sample. You can find YFSoft SDK <a href="https://pan.baidu.com/s/1i5uoKKH">here</a>:

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Provide a power supply for device, either through USB device socket or using provided external terminal connector (9~24V DC, 0.5A).

<a name="Build"></a>
# Step 3: Build and Run the Sample

<a name="Step_3_1_Connect"></a>
## 3.1 Connect the Device

-   Connect the board to your network using an Ethernet cable. This step is required, as the sample depends on internet access.

-   Plug the device into your computer using a micro-USB cable.

<a name="Step_3_2_Build"></a>
## 3.2  Build the Samples

-   Start a new instance of Visual Studio 2015. Open the **iothub_csharp_netmf_client.sln** solution (/azure-iot-sdk-csharp) from your local copy of the repository.

-   Download pre-configured project <a href="http://www.yfiot.com/Azure/YFSoft_Azure_HTTPs.rar">here</a>

-   In Visual Studio, from **Solution Explorer**, navigate to the **NetMFDeviceClientHttpSample_42 version>** project.

-   Locate the following code in the **Program.cs** file:

        public const string DeviceConnectionString = "<replace>";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   To build and deploy the binaries on your device, right-click on the project in the **Solution Explorer**, select **Properties** and navigate to the **.NET Micro Framework** tab.

    Select the **Transport** and **Device** which you have connected.

-   Build the solution.

<a name="Step_3_3_Run"></a>
## 3.3 Run and Validate the Samples

### 3.3.1 Send Device Events to IoT Hub

-   In Visual Studio, from **Solution Explorer**, right-click the sample project, click **Debug &minus;&gt; Start new instance** to build and run the sample.

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

### 3.3.2 Receive messages from IoT Hub

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
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

