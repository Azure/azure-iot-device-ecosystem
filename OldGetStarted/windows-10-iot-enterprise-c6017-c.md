---
platform: windows 10 iot enterprise
device: c6017
language: c
---

Run a simple C sample on C6017 device running Windows 10 IoT Enterprise
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build SDK and Run the sample](#BuildSDK)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect a C6017 device running Windows with Azure IoT Hub. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Install and configure TwinCAT IoT Data Agent

In addition and optionally, this document also describes how to build the Azure IoT SDK and run the sample. 
You only require this step if you do not want to use the Beckhoff TwinCAT IoT Data Agent product and instead want to build your own connector application.

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Attach the C6017 to your network by using the integrated Ethernet port
-   Ensure that the C6017 retrieved an IP address from a DHCP server

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

## Configure C6017 for Azure IoT Hub by using TwinCAT IoT Data Agent

-   Install TF6720 TwinCAT IoT Data Agent
-   Open the Data Agent Configurator tool and add a new Azure IoT Hub gate
-   Configure the gate with the device credentials from your Azure IoT Hub instance
-   Add an ADS or OPC UA gate to the configuration
-   Select the variables that should be communicated with Azure IoT Hub from the target browser
-   Select parameters like sampling rate and data format
-   Activate the configuration

<a name="BuildSDK"></a>
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

## Documentation

The TwinCAT IoT Data Agent documentation can be found [here](http://infosys.beckhoff.com).

[Manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[Save IoT Hub messages to Azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[Use Power BI to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[Remote monitoring and notifications with Logic Apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
