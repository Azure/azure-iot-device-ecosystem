---
platform: custom rtos
device: cas gateway
language: c
---

Run a simple C sample on a CAS Gateway device running Custom RTOS
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Run)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect a CAS Gateway device running Custom RTOS with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub](https://github.com/azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md)
-   [Provision your device and get its credentials](https://github.com/azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md).
-   Using the Device Manager, generate a SAS token for the device. 
-   CAS Gateway device
-   Power cable and ethernet cable for CAS Gateway
-   Null modem cable
-   Computer with a serial port

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Connect null modem cable to Port 1 on the CAS Gateway device and to the serial port on the computer.
-   Connect ethernet cable to the CAS Gateway device to add it to the local network.
-   Power up the CAS Gateway device.
-   Send Azure IoT device connection string including the SAS token to Chipkin Automation Systems for firmware file generation.
-   Receive azure iot sample firmware from Chipkin Automation Systems.  The firmware file has a file extension of .s19.
-   Download the [Auto-Update tool](http://www.chipkin.com/files/resources/CASGatewayAutoUpdate_2015Jul23153320.zip) to load firmware from the computer to the CAS Gateway device.
-   Make sure you have web access to the CAS Gateway device from the computer.

    ***Note:*** *The default IP Address of the CAS Gateways is 192.168.1.113*

<a name="Run"></a>

# Step 3: Load and Run the sample

## 3.1 Load SDK and sample

-   Open a PuTTY session and make a serial connection to the device.

        - Use the following connection settings:
                Baud rate: 115200
                Data bits: 8
                Parity: None
                Stop bits: 1

-   Download the provided firmware to the CAS Gateway device.  The firmware implements the Microsoft Azure IoT Device SDK and the simple MQTT example.  For instructions on loading the firmware, refer [here](http://www.chipkin.com/cas-gateway-firmware-download-tool/).

    ***Note:*** *After loading the firmware, the CAS Gateway device will automatically reboot and run the sample application*

## 3.2 Send Device Events to IoT Hub:

-   When the CAS Gateway device reboots, the sample application will automatically run and will start sending messages to the IoT Hub.

-   See [Manage IoT Hub](https://github.com/neeraj-khanna/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md) to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   Use the Device Manager to send messages to the device.

-   Sending 'quit' will stop the application.

-   See [Manage IoT Hub](https://github.com/neeraj-khanna/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md) to learn how to send cloud-to-device messages to the application.

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
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
