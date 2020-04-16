---
platform: ubuntu 16.04
device: ml600
language: python
---

Run a simple PYTHON sample on your ML600 device running Ubuntu 16.04
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

This document describes how to connect your ML600 device running Ubuntu 16.04 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-python]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   ML600 device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-   If you did not purchase your device with Ubuntu installed, [download and install Ubuntu 16.04](https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-desktop?_ga=2.171335648.1592099568.1506607676-222786697.1488985661#0).

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

-   Open a PuTTY session and connect to the device.

-   Install the prerequisite packages for the Microsoft Azure IoT Device SDK for Python by issuing the following commands from the command line on your board:

        sudo apt-get update

        sudo apt-get install -y curl libcurl4-openssl-dev libssl-dev build-essential cmake git python2.7-dev libboost-python-dev

-   Download the Microsoft Azure IoT Device SDK to the board by issuing the following command on the board::

        git clone --recursive https://github.com/Azure/azure-iot-sdk-python.git

-   Run following commands to build the SDK:

        cd python/build_all/linux
	    sudo ./build.sh    

-   After a successful build, the `iothub_client.so` Python extension module is copied to the **python/device/samples** folder.

- Navigate to samples folder by executing following command:

        cd azure-iot-sdk-python/device/samples/

-   Edit the following file using any text editor of your choice:

        nano iothub_client_sample_http.py

## 3.2 Send Device Events to IoT Hub:

-   Run the sample application using the following command:

        python iothub_client_sample_http.py

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

