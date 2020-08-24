---
platform: windows 10 iot core
device: asus pe100a
language: python
---

Run a simple python sample on ASUS PE100A device running Windows 10 IoT Core
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect ASUS PE100A device running Windows 10 IoT Core with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Run Azure IoT sample on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Install Windows 10 IoT Core Dashboard on PC][Windows 10 IoT Core Dashboard]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   ASUS PE100A device

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Connect PE100A to internet
-   [Install remote PowerShell][Using PowerShell for Windows IoT]
-   [Install Python][Install Python for Windows IoT]
-   Install Azure IoT Hub Python SDKs:

			python -m pip install azure-iot-device

-   Copy [python sample][Python Smaple] to device

<a name="Build"></a>
# Step 3: Run the sample

<a name="Load"></a>
## 3.1 Send Device Events to IoT Hub

-   Set the connection string for your device to powershell local variable:

		Set-Variable -Name "IOTHUB_DEVICE_CONNECTION_STRING" -Value "[device connection string]"

-   Run the sample application using the following command:

       	python send_message.py

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.2 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that sends messages to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

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
[Windows 10 IoT Core Dashboard]: https://docs.microsoft.com/zh-tw/windows/iot-core/connect-your-device/iotdashboard
[Using PowerShell for Windows IoT]: https://docs.microsoft.com/zh-tw/windows/iot-core/connect-your-device/powershell
[Install Python for Windows IoT]: https://docs.microsoft.com/zh-tw/windows/iot-core/developer-tools/python
[Python Smaple]:https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/async-hub-scenarios/send_message.py