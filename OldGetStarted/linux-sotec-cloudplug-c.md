---
platform: customised embedded LINUX distribution built with YOCTO
device: SOTEC CloudPlug
language: c
---

Run a simple C sample on SOTEC CloudPlug device running on a customised embedded LINUX distribution built with YOCTO
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Tips](#tips)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect SOTEC CloudPlug device running on a customised embedded LINUX distribution built with YOCTO with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Computer with Git client installed and access to the
    [azure-iot-sdk-c](https://github.com/Azure/azure-iot-sdk-c) GitHub
    public repository.
-   SOTEC CloudPlug device
-   LINUX VM-Instance 
-   ECLIPSE running on the LINUX VM-Instance 
-   Cross compiler settings from YOCTO 
-   Download and install [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-c/releases).
-   [Set up your IoT hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md).
#### Create a device on IoT Hub
-   With your IoT hub configured and running in Azure, follow the instructions in **"Create Device"** section of [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md).
#### Write down device credentials
-   Make note of the Connection String for your device by following the instructions in **"Get device connection string or configuration data"** section of [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md).

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-	Using the µUSB as power supply for the CloudPlug
-	Plug in the JR45-network connect to get access to the CloudPlug device using the defined IP-Address
- 	Modify the default IP-Address 192.168.1.41 using the Web-Configuration of the CloudPlug

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

-   The SDK is already part of the CloudPlug LINUX-System

-   Download the Microsoft Azure IoT Device SDK for C to your LINUX development VM by issuing the following command:

        git clone https://github.com/Azure/azure-iot-sdk-c.git

- 	Open a ECLIPSE Project and create a new Hello-World C++-Project
	Creating a Hello World C++-Project with the name iothub_client_sample_amqp is the fastest way to get a project and make the extensions for the azure IoT example.

-   Set up the cross compiler setting
	
-   Copy the Sample Code found in azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp.c into main.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Step-1:-Prerequisites) and save the changes.

-   Compile the iothub_client_sample_amqp example using the ECLIPSE project


## 3.2 Send Device Events to IoT Hub:

-	Setup the Debug configuration:
    * C Apllication: "Debug/iothub_client_sample_amqp"
	* Project "iothub_client_sample_amqp"
	* Connection: "Your IP-Address"
	* Remote absolute path: "/opt/cloudplug/bin/iothub_client_sample_amqp" 	
	
-   Run the sample by starting the configured iothub_client_sample_amqp debug session.

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

<a name="tips"></a>

# Tips
  Use always the ECLIPSE cross compiler settings for the CloudPlug from the customised embedded LINUX distribution built with YOCTO.
  More information regarding the CloudPlug can be found on www.sotec.eu


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
