---
platform: windows 10 iot enterprise 2016 ltsb
device: rail pc
language: c
---

Run a simple C sample on Rail PC device running Windows 10 IoT Enterprise 2016 LTSB
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

This document describes how to connect Rail PC device running Windows 10 IoT Enterprise 2016 LTSB with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-windows]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Rail PC device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

Windows 10 Iot Enterprise 2016 is installed

-   Connect the power adapter, optinal USB Keyboard/Mouse and a Monitor with Rail PC device.
-   Wait until the operating system is ready and connected to the Internet

or Install Windows 10 Iot Enterprise 2016 on Rail PC

-   Have an look to S&T Flatman Eco
-   Install Windows 10 Enterprise 2016 LTSB
-   Install I/O Driver
-   Verify I/O functions, especially the network

In this section, you will register your device using DeviceExplorer. The DeviceExplorer is a Windows application that interfaces with Azure IoT Hub and can perform the following operations:

-   Device management
    -   Create new devices
        -   List existing devices and expose device properties stored on Device Hub
        -   Provides ability to update device keys
        -   Provides ability to delete a device
    -   Monitoring events from your device
    -   Sending messages to your device

To run DeviceExplorer tool, use your IoT Hub connection string generated from the Azure web portal

-   IoT Hub Connection String

**Steps:**

   1. Click here to download and install DeviceExplorer.

   2.  Add connection information under the Configuration tab and click the Update button.

   3. Create and register the device with your IoT Hub using instructions as below.

      a. Click the Management tab.

      b. Your registered devices will be visible in the list. In case your device is not there in the list, click Refresh button. If this is your first time, then you shouldn't retrieve anything.

      c. Click Create button to create a device ID and key.

      d. Once created successfully, device will be listed in DeviceExplorer.

      e. Right click the device and from context menu select "Copy connection string for selected device".

      f. Save this information in Notepad. You will need this information in later steps.


<a name="Build"></a>
# Step 3: Build SDK and Run the sample

-   Start a new instance of Visual Studio 2015. Open the **azure\_iot\_sdks.sln** solution in the **cmake** folder in your home directory.

-   In Visual Studio, in **Solution Explorer**, navigate to **simplesample\_amqp** project, open the **simplesample\_amqp.c** file.

-   Locate the following code in the file:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

-   In **Solution Explorer**, right-click the **simplesample\_amqp** project, click **Debug**, and then click **Start new instance** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.

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
[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

