---
platform: mentor embedded linux
device: mf0200 iot intelligent connected gateway
language: c
---

Run a simple C sample on MF0200 IoT Intelligent Connected Gateway device running Mentor Embedded Linux
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

This document describes how to connect MF0200 IoT Intelligent Connected Gateway device running Mentor Embedded Linux with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   MF0200 IoT Intelligent Connected Gateway device.
-   A computer with Mentor Embedded Linux for MF0200 IoT Intelligent Connected Gateway installed. 
-   Terminal Emulator software.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-   On the host machine with MEL for MF0200 installed, create a build directory using the MEL setup-environment script and include the azure layer:

        <mel-dir>/meta-mentor/setup-environment -l azure mx6q-csp

-   Find the **AZURE_CONNECTION_STRING** variable in ***&lt;build&gt;/local.conf*** file and replace the placeholder with device connection string.

        AZURE_CONNECTION_STRING = “<HostName=...>”

-   Follow MEL instructions to build the image (core-image-sato) and prepare the SD card.

-   Proceed with booting the target and the demo applications will be present on the target root file system at the following location:

        /home/root/azure-iot-sdk-c/


<a name="Build"></a>
# Step 3: Build and Run the sample

## Send Device Events to IoT Hub:

-   Run the sample by issuing following command:

    **If using AMQP protocol:**

        /home/root/azure-iot-sdk-c/iothub_client_sample_http

    **If using HTTP protocol:**

        /home/root/azure-iot-sdk-c/iothub_client_sample_amqp

    **If using MQTT protocol:**

        /home/root/azure-iot-sdk-c/iothub_client_sample_mqtt

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## Receive messages from IoT Hub

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
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

