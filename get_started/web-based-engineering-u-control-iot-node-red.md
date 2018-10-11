---
platform: web-based engineering
device: u-control iot
language: node-red
---

Run a simple Node-RED sample on u-control IoT device running inside a browser
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Create and deploy a Node-RED flow to Azure IoT Hub](#Deploy)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect u-control IoT device running Node-RED with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Node-RED flow on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Computer with installed browser
-   u-control IoT device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Carefully read the u-control IoT manual
-   If neccessary, connect input and output modules to u-control IoT
-   Connect the u-control IoT device with a power supply
-   Open the u-control IoT WebUI in your browser (default IP address is 192.168.0.101)

<a name="Deploy"></a>
# Step 3: Create and deploy a Node-RED flow to send data to Azure IoT Hub

-   Inside the WebUI open the "Cloud configuration" tab, which loads Node-RED
-   Copy the following code into your clipboard and import it via "Import / Clipboard" command:

        [{"id":"9c7c3199.7f8ca","type":"azureiothub","z":"208f7193.5b1c3e","name":"Azure IoT Hub","protocol":"http","x":300,"y":240,"wires":[["c518a697.eed6a8"]]},{"id":"910f38bb.51e118","type":"inject","z":"208f7193.5b1c3e","name":"Inject data","topic":"","payload":"{\"deviceId\":\"yourDevice\",\"key\":\"yourKey\",\"protocol\":\"http\",\"data\":\"{tem: 25, wind: 20}\"}","payloadType":"json","repeat":"","crontab":"","once":false,"onceDelay":"","x":120,"y":200,"wires":[["9c7c3199.7f8ca"]]},{"id":"c518a697.eed6a8","type":"function","z":"208f7193.5b1c3e","name":"Convert Bytes to String","func":"msg.payload = msg.payload.toString();\nreturn msg;","outputs":1,"noerr":0,"x":530,"y":280,"wires":[["a185951e.085ca8"]]},{"id":"a185951e.085ca8","type":"debug","z":"208f7193.5b1c3e","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","x":750,"y":320,"wires":[]}]

-   Double click on the inject node and fill in the correct credentials for the payload
-   Double click on the Azure IoT Hub node and fill in the correct hostname from Step 1
-   Deploy the flow by clicking the "Deploy" button
-   Click on the rectangle on the left side of the inject button to send data to the Azure IoT Hub

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
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

