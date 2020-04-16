---
platform: linux
device: toradex modules
language: javascript
---

Run a simple Node sample on Toradex Modules device running Linux
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

This document describes how to build and run the **simple_sample_http.js** Node.js sample application. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Ensure the module is flashed with [Toradex V2.5 Linux image or newer][toradex_image_update].

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

1.  Insert the module into a compatible carrier board.  Power on the system and connect to the internet.

2.  Use the command line to install Node.js and necessary dependencies:

        opkg update
        opkg install nodejs nodejs-npm python python-simplejson

3.  Use the Node.js package manager (npm) to install the **azure-iot-device** library, the [sample Node.js device applications][node-sample-apps] and the sample applications' other dependencies:

        npm install azure-iot-device
        cd node_modules/azure-iot-device/samples
        npm install

<a name="Build"></a>
# Step 3: Build and Run the sample

1.  On the host machine, start a new instance of the [Device Explorer][device-explorer], select or create a new device, obtain and note the connection string for the device, and begin monitoring under the Data tab.

2.  Back on the device, at the Azure IoT Device samples location (node_modules/azure-iot-device/samples), edit the example javascript file(s) (e.g. simple_sample_http.js):

        vi simple_sample_http.js

    And locate the following line to change [IoT Device Connection String] to your connection string as provided by your IoT hub instance:

        var connectionString = '[IoT Device Connection String]';

3.  Execute the sample application using Node.js:

        node simple_sample_http.js

    The sample application will send messages to your IoT hub and the Device Explorer utility will display the messages as your IoT hub receives them.

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
[device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/readme.md
[toradex_image_update]: http://developer.toradex.com/knowledge-base/how-to-setup-environment-for-embedded-linux-application-development#Linux_Image_Update
[node-sample-apps]: https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples

[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

