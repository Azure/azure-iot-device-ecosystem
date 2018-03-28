---
platform: notouch os
device: citrix ready workspace hub by ncomputing
language: c
---

Run a simple C sample on Citrix Ready Workspace Hub by NComputing device running NoTouch Operating System
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Using the Sample](#Usethesample)
-   [Step 3: Supported Architectures](#SupportedArchitecture)
-   [Licensing](#Licensing)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

Stratodesk's azure-iot-c project provides a sample application compiled with the Microsoft Azure's azure-iot-sdk-c libraries. This project assumes that it's build environment already contains both azure-c-shared-utility and azure-iot-sdk-c, as well as system-level runtime dependencies. These dependencies are already provided within Stratodesk NoTouch OS.
 
This sample is to strictly be built for and included in Stratodesk's NoTouch Operating System.
 
***Note:*** NoTouch OS does not include the IoT Hub Device Explorer, because it does not depend on the device being tested. To receive a log of events, install the Device Explorer on another operational computer.
 
<a name="Prerequisites"></a>
# Step 1: Prerequisites
 
-   NoTouch OS (version >= 2761)
-   WAN Connection (on NoTouch OS)
 
<a name="Usethesample"></a>
# Step 2: Using the Sample
 
-   Gain access to a shell on NoTouch OS with either of these methods
    -   Navigate to `Console` in the Configuration utility
    -   SSH into the NoTouch OS session (recommended)
        -   username: notouchadm
        -   password: <your\_configured\_admin\_password> (default: admin)
-   Execute the azure-iot-c binary by running

        /opt/azure-iot-c/app (--send|--recv) 'device_connection_string'
 
**Example**

     [notouchadm@NTD82FA62340912]$ /opt/azure-iot-c/app --send '<device_connection_string>'
     ... output of send messages to IoT Hub ...
     [notouchadm@NTD82FA62340912]$ /opt/azure-iot-c/app --recv '<device_connection_string>'
     ... output of messages received from IoT Hub ...
 
<a name="SupportedArchitecture"></a>
# Step 3: Supported Architectures
 
This sample has been compiled for and exists on `i386`, `amd64`, and `armhf` architectures of the Linux operating system.
 
<a name="Licensing"></a>
# Licensing
 
The base functionality of this sample is derived from iot-hub-c-raspberrypi-sample, however, the specific alterations of iot-hub-c-rasperrypi-sample and build of this application belongs to Stratodesk Corporation, under the following copyright:
 
    Copyright &copy; Stratodesk Corporation 2018
    All Rights Reserved.

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