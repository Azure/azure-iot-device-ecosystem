---
platform: hpe-azure-certified-windows-operating-system
device: el10
language: csharp
---

Run a simple sample on EL10 device running HPE Azure Certified Windows Operating System
===
---

# Table of Contents

-  [Introduction](#Introduction)
-  [Step 1: Prerequisites](#Prerequisites)
-  [Step 2: Prepare your Device](#PrepareDevice)
-  [Step 3: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document provides step-by-step guidance on how to connect an EL10 device running **a HPE Azure Certified Windows Operating
System** with Azure IoT SDK. A HPE Azure Certified Windows operating system is one in which a number of tests were run with the 
EL10 device and the test results were submitted to Microsoft for certification. These include Windows 8.1 Embedded Industry Enterprise,
Windows 10, Windows Server 2012R2, and Windows Server 2016. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

-    Computer with GitHub installed and access to the
    [azure-iot-sdk-csharp](https://github.com/Azure/azure-iot-sdk-csharp) GitHub
    private repository.
-   EL10 device.
-   Install any version of [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
-   Install [Microsoft Azure SDK](http://www.microsoft.com/download/details.aspx?id=48178).
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-    An empty USB drive.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
##  Install Windows Operating System on the EL10
-   Obtain an iso for the specific operating system and copy it to the USB drive,
-   Make the USB drive bootable. Follow this guide on how to create a bootable drive (<https://www.microsoft.com/en-us/download/windows-usb-dvd-download-tool>),
-   Insert the bootable USB Drive from the previous step into the EL10,
-   Turn on the EL10 device and press the **Delete** key to enter the BIOS settings,
-   Change the BIOS Boot option filter to **UEFI and Legacy**,
-   Change the **Boot Option Priorities** to boot from the USB Drive,
-   Save changes and restart the EL10,
-   Follow the on screen instructions to install Windows Operating System on the EL10

<a name="Build"></a>
# Step 3: Build and Run the sample

-   Download the [Azure IoT SDK](https://github.com/Azure/azure-iot-sdk-csharp) and save them to your local repository.
-   Start a new instance of Visual Studio 2015.
-   Open the **iothub\_csharp\_deviceclient.sln** solution in the `device` folder in the local copy of the repository,
-   In Visual Studio, from Solution Explorer, navigate to the **samples** folder.
-   In the **DeviceClientAmqpSample** project, open the ***Program.cs*** file.
-   Locate the following code in the file:

        private const string DeviceConnectionString = "<replace>";
        
-   Replace `<replace>` with the connection string for your device.
-   In visual Studio, under Solution Explorer, right-click the **DeviceClientAmqpSample** project, click ***Debug &minus;&gt; Start new instance*** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.
-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application and how to send cloud-to-device messages to the application.

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
