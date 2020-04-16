---
platform: yocto linux
device: lantronix sgx5150
language: python
---

Run a simple PYTHON sample on the Lantronix SGX5150 device running Yocto Linux
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

This document describes how to connect the Lantronix SGX5150 IIoT Gateway device
running Yocto Linux with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/get_started/python-devbox-setup.md)
-   [Setup your IoT hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md)
-   [Provision your device and get its credentials](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md)
-   Lantronix SGX5150 IoT Gateway Device

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Follow the instructions in the SGX 5150 Quick Start Guide and SGX 5150 User Guide found at  [https://www.lantronix.com/products/sgx-5150/#docs-downloads](https://www.lantronix.com/products/sgx-5150/#docs-downloads) to login and connect the device with an appropriate wired or wireless network allowing the device to access the internet.

-   Follow the instructions at [https://github.com/Lantronix/yocto_premierwave](https://github.com/Lantronix/yocto_premierwave) to:
    -   Install the Yocto SDK on your Linux host
    -   Build the SGX5150 ROM image

-   Flash this newly-built ROM image to your SGX5150. Follow the instructions to:

    -   “Upload New Firmware” in the SGX 5150 User Guide at:  [https://www.lantronix.com/products/sgx-5150/#docs-downloads](https://www.lantronix.com/products/sgx-5150/#docs-downloads)

<a name="Build"></a>
# Step 3: Build and Run the Sample
## 3.1 Build SDK and sample

-   Follow the instructions at [https://github.com/Lantronix/yocto_premierwave](https://github.com/Lantronix/yocto_premierwave) to:
  -   Add Azure libraries to install the prerequisite packages for the Microsoft Azure IoT Device SDK on your Linux host.
  -   Build the appropriate files.

-   The example files have been pre-built for the SGX 5150 and are located at:

    <https://github.com/Lantronix/yocto_premierwave/tree/master/sources/meta-lantronix/recipes-connectivity/azure-iot/files>

-   The appropriate example files needed are:

    -   iothub_client.so
    -   libboost_python.so.1.58.0

-   Open a PuTTY session and connect to the SGX5150 device. Copy the following pre-built files to your SGX5150 Device by issuing the following commands from the command line:

    `scp iothub_client.so root@device_ip:/ltrx_user/`
    `scp libboost_python.so.1.58.0 root@device_ip:/usr/lib/`


-   Edit the following file using any text editor of your choice:

    **For HTTP protocol:**

    `nano iothub_client_sample_http.py`

-   Find the following place holder for device connection string:

    `connectionString = "[device connection string]"`

-   Replace the above placeholder with the device connection string you obtained in Step 1 and save the changes.

-   From your PuTTY session, copy your modified sample file to the SGX device:

    `scp iothub_client_sample_http.py root@device_ip:/ltrx_user/`

## 3.2 Send device events to IoT Hub

-   Run the sample application using the following command on the SGX 5150:

    **For HTTP protocol:**

    `/usr/bin/python2 /ltrx_user/iothub_client_sample_http.py`

-   See [Manage IoT Hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md) to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md) to learn how to send cloud-to-device messages to the application.

<a name="NextSteps"></a>
## Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer]
-   [Save IoT Hub messages to Azure data storage]
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub]
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]
-   [Remote monitoring and notifications with Logic Apps]

[Manage cloud device messaging with iothub-explorer]:(https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
[Save IoT Hub messages to Azure data storage]:(https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
[Use Power BI to visualize real-time sensor data from Azure IoT Hub]:(https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]:(https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
[Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]:(https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
[Remote monitoring and notifications with Logic Apps]:(https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)
