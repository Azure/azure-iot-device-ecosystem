---
platform: openwrt
device: bb device family
language: c

---
Run a simple C sample on BB device family running OpenWrt
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

This document describes how to connect BB device family running Linux with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment](https://catalog.azureiotsuite.com/docs?title=Azure/azure-iot-sdk-c/doc/devbox_setup)
-   [Setup your IoT hub](https://catalog.azureiotsuite.com/docs?title=Azure/azure-iot-device-ecosystem/setup_iothub)
-   [Provision your device and get its credentials](https://catalog.azureiotsuite.com/docs?title=Azure/azure-iot-device-ecosystem/manage_iot_hub)
-   Device from BB family.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Install Linux on your device.

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Step_3_1:_Build"></a>
## 3.1 Build SDK and sample

-   Open a PuTTY session and connect to the device.
-   Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by issuing the following commands from the command line on your board:

        sudo apt-get update
        sudo apt-get install -y curl libcurl4-openssl-dev uuid-dev uuid g++ make cmake git unzip openjdk-7-jre

-   Download the Microsoft Azure IoT Device SDK for C to the board by issuing the following command on the board:

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Edit the following file using any text editor of your choice:

    **For AMQP protocol:**

        azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp.c

    **For HTTPS protocol:**

        azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http.c

    **For MQTT protocol:**

        azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Build the SDK using following command.

        sudo ./azure-iot-sdk-c/build_all/linux/build.sh

<a name="Step_3_2:_Send"></a>
## 3.2 Send Device Events to IoT Hub:

-   Run the sample by issuing following command:
   
    **If using AMQP protocol:**

        ~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp

    **If using HTTP protocol:**

        ~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http

    **If using MQTT protocol:**

        ~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt

-   See [Manage IoT Hub](https://catalog.azureiotsuite.com/docs?title=Azure/azure-iot-device-ecosystem/manage_iot_hub) to learn how to observe the messages IoT Hub receives from the application.
 
<a name="Step_3_3:_Receive"></a>
##3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub](https://catalog.azureiotsuite.com/docs?title=Azure/azure-iot-device-ecosystem/manage_iot_hub) to learn how to send cloud-to-device messages to the application.



<a name="tips"></a>
# Tips

-   If you just want to build the serializer samples, run the following commands:

        •   cd ./c/serializer/build/linux
        •   make -f makefile.linux all


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

