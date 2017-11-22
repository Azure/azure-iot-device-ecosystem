---
platform: windows 10 iot enterprise ltsb
device: flatclient
language: csharp
---

Run a simple Csharp sample on Kontron FlatClient device running Windows 10 IoT Enterprise LTSB
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

This document describes how to connect Kontron FlatClient device running Windows 10 IoT Enterprise LTSB with Azure IoT SDK. This multi-step process includes:
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
-   Kontron FlatClient device.

#### Install Visual Studio 2015 and Tools

-   To create Windows IoT Core solutions, you will need to install [Visual Studio 2015](https://www.visualstudio.com/en-us/products/vs-2015-product-editions.aspx). You can install any edition of Visual Studio, including the free Community edition.

    Make sure to select the **Universal Windows App Development Tools**, the component required for writing apps Windows 10:

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Connect all cables to your FlatClient; refer to the users manual at [www.kontron.com](http://www.kontron.com)
-   Connect the power adapter (24V DC) with Kontron FlatClient
-   Wait until the operating system is ready.

<a name="Build"></a>
# Step 3: Build and Run the Sample

<a name="Step_3_1:_Build"></a>
## 3.1  Build the Samples

-   Download the Azure IoT SDK and the sample programs and save them to your local repository.

-   Start a new instance of Visual Studio 2015. Open the **iothub\_csharp\_deviceclient.sln** solution (/azure-iot-sdk-csharp) from your local copy of the repository.

-   In Visual Studio, from **Solution Explorer**, navigate to the **Samples** project.

-   Locate the following code in the **program.cs** file:

        public const string DeviceConnectionString = "<replace>";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Build the sample.
    
-   To deploy the binaries on your device by using a USB memory stick (binary application).

<a name="Step_3_2:_Run"></a>
## 3.2 Run and Validate the Samples

### 3.2.1 Send Device Events to IoT Hub

-   Start the Windows explorer and execute the application program under \bin.
-   Messages will be sent to the IoT Hub immediately.
-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

### 3.2.2 Receive messages from IoT Hub

-   Open the IoT Hub / Device Explorer.
-   Enable the corresponding Device ID and open Window 'Message to Device'.
-   Add a message and press the ‘Send Message’ button. The device should display the message.
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

