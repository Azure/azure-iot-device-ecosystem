---
platform: {Windows 10}
device: {HBFBU691-3455-B}
language: {Node.js}
---

Connect {HBFBU691-3455-B} device to your Azure IoT services
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

- Connect the power adapter, USB Keyborad/Mouse with [HBFBU691-3455-B](https://www.jetwayipc.com/product/hbfbu691-3455-b-series/) .
- Wait until the operating system is ready.

# Step 3: Build and Run the sample
Make sure you've [setup your environment](https://docs.microsoft.com/en-us/azure/iot-pnp/set-up-environment), including your IoT hub, before continuing.
To complete this quickstart, you need Node.js on your development machine. You can download the latest recommended version for multiple platforms from [nodejs.org](https://nodejs.org/).

You can verify the current version of Node.js on your development machine using the following command:

node --version

# Download the code
In this quickstart, you prepare a development environment you can use to clone and build the Azure IoT Hub Device SDK for Node.js.
Open a command prompt in the directory of your choice. Execute the following command to clone the [Microsoft Azure IoT SDK for Node.js](https://github.com/Azure/azure-iot-sdk-node) GitHub repository into this location:

git clone https://github.com/Azure/azure-iot-sdk-node

# Install required libraries
You use the device SDK to build the included sample code. The application you build simulates a device that connects to an IoT hub. The application sends telemetry and properties and receives commands.

1. In a local terminal window, go to the folder of your cloned repository and navigate to the /azure-iot-sdk-node/device/samples/pnp folder. Then run the following command to install the required libraries:

npm install

2. Configure the environment variable with the device connection string you made a note of previously:

set IOTHUB_DEVICE_CONNECTION_STRING=<"YourDeviceConnectionString">
  
# Run the sample device
This sample implements a simple IoT Plug and Play thermostat device. The model this sample implements doesn't use IoT Plug and Play components. The DTDL model file for the thermostat device defines the telemetry, properties, and commands the device implements.

Open the simple_thermostat.js file. In this file, you can see how to:

- const provisioningHost = "global.azure-devices-provisioning.net";
- const idScope = "your_idScope "
- const registrationId = "your_registrationId"
- const symmetricKey = "your_symmetricKey"
- const useDps = "PS"

To learn more about the sample configuration, see the [sample readme](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/pnp/readme.md).
Run the sample application to simulate an IoT Plug and Play device that sends telemetry to your IoT hub. To run the sample application, use the following command:

node simple_thermostat.js

You see the following output, indicating the device has begun sending telemetry data to the hub, and is now ready to receive commands and property updates.

# Next Steps

Please refer to the below link for additional information for Plug and Play 

-   [Manage cloud device messaging with Azure-IoT-Explorer](https://github.com/Azure/azure-iot-explorer/releases)
-   [Import the Plug and Play model](https://docs.microsoft.com/en-us/azure/iot-pnp/concepts-model-repository)
-   [Configure to connect to IoT Hub](https://docs.microsoft.com/en-us/azure/iot-pnp/quickstart-connect-device-c)
-   [How to use IoT Explorer to interact with the device ](https://docs.microsoft.com/en-us/azure/iot-pnp/howto-use-iot-explorer#install-azure-iot-explorer)   
