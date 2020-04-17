---
platform: linux 4.16.7
device: bte device
language: javascript
---

Run a simple JavaScript(Node) sample on BTE device running Linux 4.16.7
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

This document describes how to connect BTE device running Linux 4.16.7 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   BTE device with power supply
-   Computer with internet access and the following software installed
   -   Git client installed git

            sudo apt-get install -y nodejs

   -   SSH client, such as PuTTY
-   Ethernet cable
-   TCP/IP network with router
-   Login credentials for BTE device
-   BTE devices

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Install Linux 4.16.7 

<a name="Build"></a>
# Step 3: Build and Run the sample
## 3.1 Build SDK and sample
-   Open a PuTTY session and connect to the device.
-   Install the prerequisite packages by issuing the following commands from the command line on the device.

         sudo apt-get install -y nodejs

-   Download the SDK to the board by issuing the following command in PuTTY:

         git clone --recursive https://github.com/Azure/azure-iot-sdk-node.git

-   To validate the source code run the following commands on the device.

        export IOTHUB_CONNECTION_STRING='<iothub_connection_string>'

    Replace the `<iothub_connection_string>` placeholder with IoTHub Connection String you got in [Step 1](#Prerequisites).


-   Run the following commands 

         cd ~/azure-iot-sdk-node
         build/dev-setup.sh
         build/build.sh | tee node-log-file.txt

-   Install npm package to run sample.

         cd ~/azure-iot-sdk-node/device/samples

    **For AMQP Protocol:**

         npm install azure-iot-device-amqp

    **For HTTP Protocol:**

         npm install azure-iot-device-http


    **For MQTT Protocol:**

         npm install azure-iot-device-mqtt	

    **For Web Sockets with AMQP Protocol:**

         npm install azure-iot-device-amqp

    **For Web Sockets with MQTT Protocol:**

         npm install azure-iot-device-mqtt
         cd ~/azure-iot-sdk-node/device/samples
         nano simple_sample_device.js

-   Find the below code:

     // Uncomment one of these transports and then change it in fromConnectionString to test other transports

         // var Protocol = require('azure-iot-device-amqp').AmqpWs;
         // var Protocol = require('azure-iot-device-http').Http;
         var Protocol = require('azure-iot-device-amqp').Amqp;
         // var Protocol = require('azure-iot-device-mqtt').MqttWs;

-   Scroll down to the connection information. Find the following place holder for IoT connection string:

         var connectionString = "[IoT Device Connection String]";

-   Press Ctrl+X to exit nano.

-   Run the following command before leaving the `~/azure-iot-sdk-node/device/samples` directory

         npm link azure-iot-device


## 3.2 Send Device Events to IoT Hub:

-   Run the sample by issuing following command:

        node ~/azure-iot-sdk-node/device/samples/simple_sample_device.js


-   See [Manage IoT Hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md) to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md) to learn how to send cloud-to-device messages to the application.

You should be able to see the command received in the console window for the client sample.


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
[setup-devbox-linux]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/get_started/node-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
