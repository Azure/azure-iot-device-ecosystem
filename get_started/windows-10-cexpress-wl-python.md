---
platform: windows 10 
device: cexpress-wl
language: python
---

Run a simple python sample on cExpress-WL COM Module with Express-BASE6 Carrier board running Windows 10 
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

This document describes how to connect cExpress-WL COM Module with Express-BASE6 Carrier board running Windows 10 Professional with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-python]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   cExpress-WL COM Module with Express-BASE6 Carrier board device.  

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Make sure the Internet is on the machine, and then execute the azure-iot connection Azure

<a name="Build"></a>
# Step 3: Build and Run the sample

## 3.1 Build SDK and sample

-   Download latest SDK using following command:

		git clone --recursive https://github.com/Azure/azure-iot-sdk-python.git

-   Ensure that the desired Python version is installed (2.7.x, 3.4.x or 3.5.x). Run python --version or python3 --version at the command line to check the version. 

-   Open a Visual Studio 2015 x86 Native Tools command prompt and navigate to the folder python/build_all/windows in your local copy of the repository.

-   Run the script build.cmd in the python\build_all\windows directory.

-   As a result, the **iothub_client.pyd** Python extension module is copied to the python/device/samples folder.

-   Navigate to the folder python/device/samples in your local copy of the repository.

-   Open the file **iothub_client_sample_amqp.py**, **iothub_client_sample_http.py** or  **iothub_client_sample_mqtt.py** in a text editor.

-   Locate the following code in the file:

		connectionString = "[device connection string]"

-   Replace [device connection string] with the connection string for your device. Save the changes.

## 3.2 Send Device Events to IoT Hub:

-   Run the sample application using the following command through Visual Studio 2015 x86 Native Tools command prompt and navigate to the folder python/build_all/windows in your local copy of the repository:

​      **If using AMQP protocol:**

	python iothub_client_sample_amqp.py

​      **If using HTTPS protocol:**

	python iothub_client_sample_http.py

​      **If using MQTT protocol:**

	python iothub_client_sample_mqtt.py

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

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
[setup-devbox-python]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/get_started/python-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
