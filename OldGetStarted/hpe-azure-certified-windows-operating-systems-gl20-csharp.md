---
platform: hpe-azure-certified-windows-operating-systems
device: hpe gl20 iot gateway
language: csharp
---

Run a simple Csharp sample on a Hewlett Packard Enterprise GL20 IoT Gateway device running a HPE Azure Certified Windows Operating System
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare the Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect a Hewlett Packard Enterprise GL20 IoT Gateway device running a HPE Azure Certified Windows Operating System with Azure IoT SDK. This multi-step process includes:
-   Configuring the  Azure IoT Hub
-   Registering the IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

Have the following items ready before beginning the process:

-   [Prepare the development environment][setup-devbox-windows]
-   [Setup the IoT hub][lnk-setup-iot-hub]
-   [Provision the device and get its credentials][lnk-manage-iot-hub]
-   Hewlett Packard Enterprise GL20 IoT Gateway device.
-   Empty USB drive

<a name="PrepareDevice"></a>
# Step 2: Prepare the Device

Install Windows Operating System on Hewlett Packard Enterprise GL20 IoT Gateway device
--------------------------------------------------------

1.  Obtain an iso for the specific operating system and copy it to the
    USB drive.

2.  Make the USB drive bootable. Follow
    [this](https://www.microsoft.com/en-us/download/windows-usb-dvd-download-tool)
    guide on how to create a bootable drive.

3.  Insert the bootable USB Drive from the previous step into the GL20
    IoT Gateway.

4.  Turn on the GL20 IoT Gateway device and press the **Delete** key to
    enter the BIOS settings.

5.  Change the BIOS Boot option filter to **UEFI and Legacy**.

6.  Change the **Boot Option Priorities** to boot from the USB Drive.

7.  Save changes and restart the GL20 IoT Gateway.

8.  Follow the on screen instructions to install the Windows Operating
    System on the GL20 IoT Gateway.

<a name="Build"></a>
# Step 3: Build and Run the sample

-   Download the [Azure IoT SDK](https://github.com/Azure/azure-iot-sdk-csharp) and the sample programs and save them to a local repository.
-   Start a new instance of Visual Studio 2015.
-   Open the **iothub\_csharp\_client.sln** solution in the `device` folder in the local copy of the repository.
-   In Visual Studio, from Solution Explorer, navigate to the **samples** folder.
-   In the **DeviceClientAmqpSample** project, open the ***Program.cs*** file.
-   Locate the following code in the file:

        private const string DeviceConnectionString = "<replace>";
        
-   Replace `<replace>` with the connection string for the device.
-   In **Solution Explorer**, right-click the **DeviceClientAmqpSample** project, click **Debug**, and then click **Start new instance** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.
-   Use the **DeviceExplorer** utility to observe the messages IoT Hub receives from the **Device Client AMQP Sample** application.
-   Refer "Monitor device-to-cloud events" in [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to see the data the device is sending.
-   Refer "Send cloud-to-device messages" in [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) for instructions on sending messages to device.

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer]
-   [Save IoT Hub messages to Azure data storage]
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub]
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
-   [Weather forecast using the sensor data from an IoT hub in Azure Machine Learning]
-   [Remote monitoring and notifications with Logic Apps]   

[Manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[Save IoT Hub messages to Azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[Use Power BI to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[Weather forecast using the sensor data from an IoT hub in Azure Machine Learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[Remote monitoring and notifications with Logic Apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

