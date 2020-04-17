---
platform: debian
device: tinker-board 
language: c
---
# Run a simple C sample on Tinker Board running Debian

***

# Table of Contents

* [Introduction](#introduction)
* [Step 1: Prerequisites](#prerequisites)
* [Step 2: Prepare your Device](#preparedevice)
* [Step 3: Build and Run the Sample](#build)
* [Next Steps](#nextsteps)

# Introduction

**About this document**
This document describes how to connect Tinker Board running Debian with Azure IoT SDK. This multi-step process includes:


* Configuring Azure IoT Hub
* Registering your IoT device
* Build and deploy Azure IoT SDK on device

# Step 1: Prerequisites

You should have the following items ready before beginning the process:

* [Prepare your development environment](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md)
* Tinker Board with Git client installed and access to the [azure-iot-sdk-c](https://github.com/Azure/azure-iot-sdk-c) GitHub public repository
* [Setup your IoT hub](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/doc/setup_iothub.md)
* [Provision your device and get its credentials](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md)


# Step 2: Prepare your Tinker Board

* Checke section 'Getting started' at [ASUS Tinker board website](https://www.asus.com/us/Single-Board-Computer/Tinker-Board/) to parepare HW/peripherals requirement and boot up procedure.
* Visit [ASUS Tinker board website](https://www.asus.com/us/Single-Board-Computer/Tinker-Board/) to setup your Tinkerboard and download Debian OS image to run on Tinker Board.
* Make sure Tinker Board is ready as per instructions given on [Prepare your development environment](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) at section **Set up a Linux development environment**


# Step 3: Build and Run the sample

## 3.1 Build SDK and sample

* Open a PuTTY session and connect to the device.
* Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by issuing the following commands from the command line on your board:

```
sudo apt-get update
sudo apt-get install -y curl libcurl4-openssl-dev build-essential cmake git libssl-dev uuid-dev
```

* Download the Microsoft Azure IoT Device SDK for C to the board by issuing the following command on the board::

```
git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git
```
* Edit the following file using any text editor of your choice:
* For AMQP protocol:
```
azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp.c
```

* For AMQP over websocket protocol:

```
azure-iot-sdk-c/iothub_client/samples/\iothub_client_sample_amqp_websockets_shared/iothub_client_sample_amqp_websockets_shared.c
```
* For MQTT protocol:
```
azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt.c
```
* For MQTT over websocket protocol:
```
azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_mqtt_websockets/iothub_client_sample_mqtt_ws.c
```

* Find the following place holder for IoT connection string and build the sample:

```
static const char* connectionString = "[device connection string]";
```
* Replace the above placeholder with device connection string you obtained in [Step 1](#prerequisites) and save the changes.
* Build the SDK using following command.

```
cd ./azure-iot-sdk-c
mkdir cmake
cmake ..
cmake --build .
```

## 3.2 Send Device Events to IoT Hub:

* Run the sample by issuing following command:
* *If using AMQP protocol:

```
~/azure-iot-sdk-c/cmake/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp
```

* If using AMQP over websocket protocol:

```
~/azure-iot-sdk-c/cmake/iothub_client/samples/iothub_client_sample_amqp_websockets/iothub_client_sample_amqp_websockets
```
* If using MQTT protocol:
```
~/azure-iot-sdk-c/cmake/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt
```
* If using MQTT over websocket protocol:
```
~/azure-iot-sdk-c/cmake/iothub_client/samples/iothub_client_sample_mqtt_websockets/iothub_client_sample_mqtt_websockets
```

* See [Manage IoT Hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md) and [How to use Device Explorer for Iot Hub devices](https://github.com/fsautomata/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to learn how to observe the messages IoT Hub **receives** from the application.

## 3.3 Receive messages from IoT Hub

* See [Manage IoT Hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md) and [How to use Device Explorer for Iot Hub devices](https://github.com/fsautomata/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to learn how to **send** cloud-to-device messages to the application.


# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

* [Manage cloud device messaging with iothub-explorer](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
* [Save IoT Hub messages to Azure data storage](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
* [Use Power BI to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
* [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
* [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
* [Remote monitoring and notifications with Logic Apps](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)
