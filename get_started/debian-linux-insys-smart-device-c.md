---
platform: debian linux
device: insys smart device
language: c
---

Run a simple C sample on INSYS Smart Device running Debian Linux
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

This document describes how to connect INSYS Smart Device running Debian Linux with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   INSYS Smart Device with Internet access, commissioned using the Startup Wizard as described in the Quick Installation Guide
-   Debian 9 (Stretch) container available in the [INSYS Knowledge Base](https://www.insys-icom.de/icom-smartbox/container)

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-   Open web interface of the router using a browser (default IP: 192.168.1.1, default user name: insys, default password: icom).
-   In the Administration -> Container menu, click on Browse in the Import container section. Select the container file and click on Import container.
-   Generate a configuration for the imported container by clicking on the magic wand symbol.
-   Configure the imported container by clicking on the pen symbol.
-   Under Bridge to IP net, select the network of the INSYS Smart Device to which your application is connected.
-   Specify an IP address for the container in this network.
-   Under User group for CLI without authentication, select Read/Write.
-   Click on Save Settings.
-   Activate the profile by clicking the blinking gear in the title bar.

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

-   Open a PuTTY session and connect to the deviceunder above specified container IP address.

-   Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by issuing the following commands from the command line on your board:

        apt-get update

        apt-get install -y curl uuid-dev libcurl4-openssl-dev build-essential cmake git

        apt-get install libssl-dev

        apt-get install git

-   Download the Microsoft Azure IoT Device SDK for C to the board by issuing the following command on the board::

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Edit the following file for sending telemetry to IoT hub using any text editor of your choice (here with nano):

        nano azure-iot-sdk-c/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Find the following place holder for editing protocol:


        // Select the Protocol to use with the connection

        #ifdef USE_AMQP

        //protocol = AMQP_Protocol_over_WebSocketsTls;

        protocol = AMQP_Protocol;

        #endif

        #ifdef USE_MQTT

        //protocol = MQTT_Protocol;

        //protocol = MQTT_WebSocket_Protocol;

        #endif

        #ifdef USE_HTTP

        //protocol = HTTP_Protocol;

         #endif

-   Uncomment the line for the MQTT protocol and comment the the line with the AMQP protocol.

-   Save your changes and exit the text editor.

-   Edit the following file for sending message from IoT hub to device using any text editor of your choice (here with nano):

        nano azure-iot-sdk-c/iothub_client/samples/iothub_ll_c2d_sample/iothub_ll_c2d_sample.c

-   Edit the file in the same way as above (connection string and protocol).

-   Build the SDK using following command.

        ./azure-iot-sdk-c/build_all/linux/build.sh 

## 3.2 Send Device Events to IoT Hub:

-   Launch the DeviceExplorer and navigate to Data tab. Select the device name you created from the drop-down list of device IDs and click Monitor button.

-   DeviceExplorer is now monitoring data sent from the selected device to the IoT Hub.

-   Run the sample by issuing following command:

        azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample

-   Verify that the confirmation messages show an OK. If not, then you may have incorrectly copied the device hub connection information.

-   DeviceExplorer should show that IoTHub has successfully received data sent by sample test.

## 3.3 Receive messages from IoT Hub

-   Run the sample by issuing following command:

        azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_ll_c2d_sample/iothub_ll_c2d_sample

-   To verify that you can send messages from the IoT Hub to your device, go to the Message To Device tab in DeviceExplorer.

-   Select the device you created using Device ID drop down.

-   Add some text to the Message field, then click Send.

-   You should be able to see the command received in the console window for the client sample.

<a name="tips"></a>
# Tips

-   If you just want to build the serializer samples, run the following commands:

        cd ./c/serializer/build/linux
        make -f makefile.linux all


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
