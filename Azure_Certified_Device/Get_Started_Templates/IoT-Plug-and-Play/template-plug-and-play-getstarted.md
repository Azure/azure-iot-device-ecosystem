---
platform: {Windows 10}
device: {HBFBU691-3455-B}
language: {Node.js}
---

Connect HBFBU691-3455-B device to your Azure IoT services
===

---
# Table of Contents

- Introduction
- Step 1: Prerequisites
- Step 2: Prepare your Device
- Step 3: Build and Run the Sample
- Next Steps

<a name="Introduction"></a>

# Introduction 

**About this document**

This document describes how to connect [HBFBU691-3455-B](https://www.jetwayipc.com/product/hbfbu691-3455-b-series/) device running Windows 10 with Azure IoT SDK. This multi-step process includes:

- Configuring Azure IoT Hub
- Registering your IoT device
- Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:
- [Prepare your development environment](https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md)
- [Setup your IoT hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md)
- [Provision your device and get its credentials](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md)
- HBFBU691-3455-B device.
- VGA Cable,USB Keyboard/Mouse, and the Internet connection

<a name="preparethedevice"></a>
# Step 2: Prepare the Device.

- Make sure the Internet is on the machine, and then execute the azure-iot connection Azure

# Step 3: Build and Run the sample

**3.1 Build SDK and sample**

- Download latest SDK using following command:

git clone https://github.com/Azure/azure-iot-sdk-node

- Install required libraries
1. In a local terminal window, go to the folder of your cloned repository and navigate to the /azure-iot-sdk-node/device/samples/pnp folder. Then run the following command to install the required libraries:

  npm install

2.Open the file simple_thermostat.js in a text editor.

  DEVICE_CONNECTION_STRING = "[device connection string]"
  
  Replace [device connection string] with the connection string for your device. Save the changes.
  
**3.2 Send Device Events to IoT Hub**

Run the sample application to simulate an IoT Plug and Play device that sends telemetry to your IoT hub. To run the sample application, use the following command:

node simple_thermostat.js

You see the following output, indicating the device has begun sending telemetry data to the hub, and is now ready to receive commands and property updates.

**3.3 Receive messages from IoT Hub**

- See [Manage IoT Hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md) to learn how to send cloud-to-device messages to the application.

# Next Steps

Please refer to the below link for additional information for Plug and Play 

-   [Manage cloud device messaging with Azure-IoT-Explorer](https://github.com/Azure/azure-iot-explorer/releases)
-   [Import the Plug and Play model](https://docs.microsoft.com/en-us/azure/iot-pnp/concepts-model-repository)
-   [Configure to connect to IoT Hub](https://docs.microsoft.com/en-us/azure/iot-pnp/quickstart-connect-device-c)
-   [How to use IoT Explorer to interact with the device ](https://docs.microsoft.com/en-us/azure/iot-pnp/howto-use-iot-explorer#install-azure-iot-explorer)   
