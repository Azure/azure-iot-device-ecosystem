---
platform: debian 8
device: artigo-a630
language: c
---

Run a simple C sample on ARTiGO-A630 running Debian 8
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

This document describes how to build ARTiGO-A630 and Starter board running Debian 8 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Sign Up To Azure IoT Hub][sign-iot-hub]
-   [Prepare ARTiGO-A630][setup-hardware]
-   [Prepare your development environment][setup-A630-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Empty microSD card 4GB at least.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

You should have following steps to prepare the device.

## 2.1 Intall Debian OS into ARTiGO-A630

-   [Here][setup-linux] is instruction how to install Debian OS and setup Linux Host PC.

## 2.2 Create projects and build images

-   [Here][setup-A630-linux] is instruction to create projects and build images.

## 2.3 Create microSD card

-	Create an FAT format partition on SD card.
-	Copy EVK image into SD card partition.
-   umount your 4GB size SD card.
-   Insert SD card into ARTiGO-A630 then power on it.
-   Remove move SD card when update finished.


## 2.4 Install necessary package

-   Account and Password is "debian"
-   Open terminal window then key-in below command

        sudo apt-get update
		sudo apt-get install curl uuid-dev libcurl4-openssl-dev build-essential cmake git libssl-dev

-   Please make sure your ARTiGO-A630 date and timezone was same as your Azure server setting

	    date 120817002017 (12/08/2017 17:00)
    	dpkg-reconfigure tzdata (change timezone)
    

## 2.5 Confirm software version

Check software version. **cmake** version is 3.0.2. **gcc** version is 4.9.2.

    cmake --version
    gcc --version

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Install SDK

-   Download Azure IoT Device SDK for C by following commands.

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git


## 3.2 Build SDK and sample

-   Add device connect string into below file by using `vim.basic`:
		
		azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp.c
		azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp_websockets/iothub_client_sample_amqp_websockets.c
		azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http.c
		azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt.c
		azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_mqtt_websockets/iothub_client_sample_mqtt_ws.c

-   Find the following place holder for device connection string:

        connectionString = "[device connection string]"

-   Run following commands to build the SDK:

        cd azure-iot-sdk-c/build_all/linux
        ./build.sh    

-   After a successful build, you can find **cmake/iotsdk_linux/iothub_client/samples** folder.

-   Navigate to samples folder by executing following command:

        cd azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

## 3.3 Send Device Events to IoT Hub:

-   Run the sample application using the following command:

    **For AMQP protocol:**

        cd azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_amqp
		./iothub_client_sample_amqp

    **For HTTP protocol:**

        cd azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_http
		./iothub_client_sample_http

    **For MQTT protocol:**

        cd azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_mqtt
		./iothub_client_sample_mqtt

	**For AMQP Websocket protocol:**

        cd azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_amqp_websockets
		./iothub_client_sample_amqp_websockets

	**For MQTT Websocket protocol:**

        cd azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_mqtt_websockets
		./iothub_client_sample_mqtt_websockets

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.4 Receive messages from IoT Hub

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
[setup-hardware]: http://cdn.viaembedded.com/products/docs/artigo-a630/User_Manual/UM_ARTiGO_A630_v1.00_20171002.pdf
[setup-A630-linux]: http://cdn.viaembedded.com/products/docs/artigo-a630/Linux_Development_Guide/ARTiGO_A630_Linux_BSP_v1.0.1_Development_Guide_v1.00_20170829.pdf
[sign-iot-hub]: https://account.windowsazure.com/signup?offer=ms-azr-0044p
[setup-linux]: http://cdn.viaembedded.com/products/docs/artigo-a630/Liunx_Quick_Start_Guide/ARTiGO_A630_Linux_EVK_v1.0.1_Quick_Start_Guide_v1.00_20170829.pdf
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
