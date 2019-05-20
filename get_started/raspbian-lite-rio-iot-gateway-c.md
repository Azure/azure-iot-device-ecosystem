---
platform: raspbian lite
device: rio iot gateway
language: c
---

Run a simple C sample on RIO IoT Gateway device running Raspbian Lite
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

This document describes how to connect RIO IoT Gateway device running Raspbian Lite with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Linux System for cross-compiling Azure IOT SDK
-   You can remotely access the command line on the Raspberry Pi via SH client (ex: Putty).
-   Required hardware:
	-   RIO IoT Gateway
	-   16GB MicroSD Card
	-   USB keyboard
	-   USB mouse (optional)
	-   HDMI cable
	-   Monitor that supports HDMI
	-   Ethernet cable or Wi-Fi dongle
-   Setup your IoT hub
-   Provision your device and get its credentials

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-   Install the latest Raspbian operating system on your RIO IoT Gateway.
-   After installation, the Raspberry Pi configuration menu is ready. You can set the date/time for your region. Under Advanced Options, enable ssh so you can access the device remotely with Putty.
-   Connect RIO device to your network using an ethernet cable or a wireless
-   You need the configure IP address of RIO device in order to connect over the network.
-   Then, you will be able to see that your device is working, 
-   Open SSH terminal program (Putty)
-   Enter IP address and Port (22) and select Connection Type as SSH 
-   When connection is done, login with username (pi) and password (raspberry)

<a name="Build"></a>
# Step 3: Build and Run the sample
Prepare the cross-compile host according to the instructions, but make sure you modify the connection string before the "Building the SDK" step.

-   Edit the file `~/Source/azure-iot-sdk-c/serializer/samples/simplesample_amqp/simplesample_amqp.c` and replace connection string placeholder with the device connection string you obtained when you provisioned your device.The device connection string should be in this format **HostName=<iothub-name>.azure-devices.net;DeviceId=<device-name>;SharedAccessKey=<device-key>**.
(You can use the console-based text editor nano to edit the file):

        static const char* connectionString = "[device connection string]";
	
	**Note:** You can skip this step if you only want to build the samples without running them.
	
-   Now, follow the instructions in the "Building the SDK" step of the instructions to build the SDK and samples.

-   Finally, create a tar containing the compiled SDK and samples.

         cd ~/Source/ ; tar czvf iotsdk_linux.tar.gz azure-iot-sdk-c/cmake/iotsdk_linux ; scp iotsdk_linux.tar.gz pi@<YOUR-RASPBIAN-HOST>:

-   change YOUR-RASPBIAN-HOST to the hostname of your RIO IoT Gateway

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
[lnk-setup-iot-hub]: ../../setup_iothub.md
[lnk-manage-iot-hub]: ../../manage_iot_hub.md
