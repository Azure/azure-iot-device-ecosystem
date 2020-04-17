---
platform: ubuntu core 16.04
device: rigado cascade-500 iot gateway
language: javascript
---

Run a simple JavaScript sample on Rigado Cascade-500 IoT Gateway device running Ubuntu Core 16.04
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

This document describes how to connect Rigado Cascade-500 IoT device running Ubuntu Core 16.04 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Windows PC with [Git client](https://git-scm.com/download/gui/windows), [WinSCP](https://winscp.net/eng/download.php) and [PuTTY](https://www.putty.org/) installed
-   [Cascade-500 Eval Kit](https://www.rigado.com/cascade-evaluation-kit/)
-   [Edge Direct](https://www.rigado.com/products/iot-edge-as-a-service/) account access granted

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Follow the vendor instructuions [Set up your Gateway](https://docs.rigado.com/en/latest/eval-kit/setting-up-gateway.html). Plug power supply and Ethernet cable

-   [Submit a request](https://rigado.zendesk.com/hc/en-us/requests/new) to Rigado. Ask to provide [developer access (ssh)](https://en.wikipedia.org/wiki/Secure_Shell) to your Gateway

-   Wait untill the LED is solid green on the Gateway

-   Login to the [Edge Direct](https://app.rigado.com) account. Go to the `GATEWAYS` tab. Click on your gateway (You can find S/N of the gateway on the sticker). Go to `NETWORK` Tab. Discover IP-adress of `eth0` and use it for connection via ssh

<a name="Build"></a>
# Step 3: Build and Run the Sample

<a name="Load"></a>
## 3.1 Load the Azure IoT bits and prerequisites on device

-   Open a WinSCP session and connect to the device.

-   Copy file [azure-iot-node-verify_1.0.0_armhf.snap](./azure-iot-node-verify_1.0.0_armhf.snap) to the device home directory.

-   Open a PuTTY session and connect to the device.

-   Install certification toolkit on the gateway. Run the following command in PuTTY:

        snap install ~/azure-iot-node-verify_1.0.0_armhf.snap --devmode -dangerous

-   Copy testing samples to home directory. Run the following command in PuTTY:

        cp -R /snap/azure-iot-node-verify/current/samples ~/samples

-   Verify that you now have a copy of the source code under the directory ~/samples.

<a name="BuildSamples"></a>
## 3.2 Build the samples

-   To validate the source code run the following commands on the device.

        export IOTHUB_CONNECTION_STRING='<iothub_connection_string>'

    Replace the `<iothub_connection_string>` placeholder with IoTHub Connection String you got in [Step 1](#Prerequisites).    

-   To update sample, run the following command on device.

        cd ~/samples
        azure-iot-node-verify.nano ./simple_sample_device.js

-   This launches a console-based text editor. Scroll down to the protocol information.
    
-   Find the below code:

        var Protocol = require('azure-iot-device-amqp').Amqp;
    
    The default protocol used is AMQP. Code for other protocols(HTTP/MQTT) are mentioned just below the above line in the script.
    Uncomment the line as per the protocol you want to use.


-   Scroll down to the connection information.
    Find the following place holder for IoT connection string:

        var connectionString = "[IoT Device Connection String]";

-   Replace the above placeholder with device connection string. You can get this from DeviceExplorer as explained in [Step 1](#Prerequisites), that you copied into Notepad.

-   Save your changes by pressing Ctrl+O and when nano prompts you to save it as the same file, just press ENTER.

-   Press Ctrl+X to exit nano.

<a name="Run"></a>
## 3.3 Run and Validate the Samples

### 3.3.1 Send Device Events to IOT Hub

-   Run the sample by issuing following command and verify that data has been successfully sent and received.

        azure-iot-node-verify.node ~/samples/simple_sample_device.js

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

### 3.3.2 Receive messages from IoT Hub

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
[lnk-setup-iot-hub]: https://docs.microsoft.com/en-us/azure/iot-hub/quickstart-send-telemetry-dotnet#create-an-iot-hub
[lnk-manage-iot-hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-device-management-overview
