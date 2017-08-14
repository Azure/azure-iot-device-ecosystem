---
platform: windows 10 iot core
device: ucom-sl6c
language: csharp
---

Run a simple Csharp sample on uCOM-SL6C device running Windows 10 IoT Enterprise 
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect uCOM-SL6C device running Windows 10 IoT Enterprise with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   uCOM-SL6C device.


<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   uCOM-SL6C is kind of computer on module. It needs to work with carriers that support uCOM module as single board computer. So. Please put uCOM-SL6C on uCOM carrier and power on it for necessary operation. 

<a name="Build"></a>
# Step 3: Build and Run the Sample


-   Download the [Azure IoT SDK][Azure IoT SDK] and the sample programs and save them to your local repository.
-   Start a new instance of Visual Studio 2015.
-   Open the iothub_csharp_client.sln solution in the (azure-iot-sdk-csharp-master\device) folder in your local copy of the repository.
-   In Visual Studio, from Solution Explorer, navigate to the samples folder.
-   In the DeviceClientAmqpSample project, open the Program.cs file.
-   Locate the following code in the file:
-   private const string DeviceConnectionString = "<replace>";
-   Replace <replace> with the connection string for your device.
-   In Solution Explorer, right-click the DeviceClientAmqpSample project, click Debug, and then click Start new instance to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.
-   Use the DeviceExplorer utility to observe the messages IoT Hub receives from the Device Client AMQP Sample application.
-   Refer "Monitor device-to-cloud events" in [DeviceExplorer Usage document][DeviceExplorer Usage] to see the data your device is sending.
-   Refer "Send cloud-to-device messages" in [DeviceExplorer Usage document][DeviceExplorer Usage] for instructions on sending messages to device.


[Manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[Save IoT Hub messages to Azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[Use Power BI to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[Remote monitoring and notifications with Logic Apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[Azure IoT SDK]:https://github.com/azure/azure-iot-sdk-csharp
[DeviceExplorer Usage]:https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md


