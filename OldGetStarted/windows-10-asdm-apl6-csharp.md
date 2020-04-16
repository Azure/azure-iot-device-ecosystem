---
platform: windows 10
device: asdm-apl6
language: csharp
---

Run a simple Csharp sample on ASDM-APL6 device running Windows 10
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Sign Up To Azure IoT Hub](#Sign Up To Azure IoT Hub )
-   [Step 2: Register & Prepare your Device](#Register Device)
-   [Step 3: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect ASDM-APL6 device running Windows 10 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

**Prepare**

Before executing any of the steps below, read through each process, step by step to ensure end to end understanding.

You should have the following items ready before beginning the process: 

-   Computer with GitHub installed and access to the [azure-iot-sdk-csharp](https://github.com/Azure/azure-iot-sdk-csharp) GitHub public repository.
-   Install Visual Studio 2015 and Tools. You can install any edition of Visual Studio, including the free Community edition. 

<a name="Sign Up To Azure IoT Hub "></a>
# Step 1: Sign Up To Azure IoT Hub 

The instructions [here](https://portal.azure.com/) on how to sign up to the Azure IoT Hub service. 
As part of the sign up process, you will receive the connection string.

-   IoT Hub Connection String: An example of IoT Hub Connection String is as below:

        HostName=[YourIoTHubName];SharedAccessKeyName=[YourAccessKeyName];SharedAccessKey=[YourAccessKey]

<a name="Register Device"></a>
# Step 2: Register Device

In this section show register your device using DeviceExplorer. The DeviceExplorer is a Windows application that interfaces with Azure IoT Hub and can perform the following operations:

-   Device management
    -   Create new devices
    -   List existing devices and expose device properties stored on Device Hub
    -   Provides ability to update device keys
    -   Provides ability to delete a device
-   Monitoring events from your device
-   Sending messages to your device
To run DeviceExplorer tool, use following configuration string as described in Step1: 
-   IoT Hub Connection String

**Steps:**

1. Click [here](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/readme.md) to download and install DeviceExplorer. 
2. Add connection information under the **Configuration** tab and click the **Update** button. 
3. Create and register the device with IoT Hub using instructions as below.
   
   -   Click the **Management** tab.
   -   Registered devices will be visible in the list. In case device is not there in the list, click **Refresh** button. If this is your first time, then you shouldn't retrieve anything. 
   -   Click Create button to **create** a device ID and key. 
   -   Once created successfully, device will be listed in DeviceExplorer. 
   -   Right click the device and from context menu select **"Copy connection string for selected device".**
   -   Save this information in Notepad. 

<a name="Build"></a>
# Step 3: Build and Run the sample

-   Download the [Azure IoT SDK](https://github.com/Azure/azure-iot-sdk-csharp) for C# and and save them to your local repository. 
-   Install Visual Studio 2015 or 2017. 
-   Install Azure SDK for .NET 
-   Start a new instance of Visual Studio 2015 or Visual Studio 2017. 
-   Open the **iothub\_csharp\_deviceclient.sln** solution in the *csharp\device* folder in your local SDK **azure-iot-sdk-csharp** directory. 
-   In Visual Studio, from **Solution Explorer.**  
-   Navigate to **DeviceClientAmqpSample** project and open the **Program.cs** file. 
-   Locate the following code in the **Program.cs.** 

        private const string DeviceConnectionString = "<replace>";

-   Replace <replace> with the connection string for your device and **Save** the changes. the connection string from DeviceExplorer. 
-   In **Solution Explorer**, right-click the **DeviceClientAmqpSample** project, click **Debug**, and then click **Start new instance** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub. 
-   Use the **DeviceExplorer** utility to observe the messages IoT Hub receives from the **Device Client AMQP Sample** application. 
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

