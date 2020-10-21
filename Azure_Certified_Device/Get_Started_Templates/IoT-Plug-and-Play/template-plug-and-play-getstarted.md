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
Make sure you've setup your environment, including your IoT hub, before continuing.
To complete this quickstart, you need Node.js on your development machine. You can download the latest recommended version for multiple platforms from [nodejs.org](https://nodejs.org/).

You can verify the current version of Node.js on your development machine using the following command:
node --version

Clone the SDK repository with the sample code
Clone the samples from a the Node SDK repository. Open a terminal window in a folder of your choice. Run the following command to clone the Microsoft Azure IoT SDK for Node.js GitHub repository:
git clone https://github.com/Azure/azure-iot-sdk-node


In Set up your environment, you created four environment variables to configure the sample to use the Device Provisioning Service (DPS) to connect to your IoT hub:

- IOTHUB_DEVICE_SECURITY_TYPE with the value DPS
- IOTHUB_DEVICE_DPS_ID_SCOPE with the DPS ID scope.
- IOTHUB_DEVICE_DPS_DEVICE_ID with the value my-pnp-device.
- IOTHUB_DEVICE_DPS_DEVICE_KEY with the enrollment primary key.
- IOTHUB_DEVICE_DPS_ENDPOINT with the value global.azure-devices-provisioning.net.
To learn more about the sample configuration, see the sample readme.

In this quickstart, you use a sample thermostat device that's written in Node.js as the IoT Plug and Play device. To run the sample device:

1.Open a terminal window and navigate to the local folder that contains the Microsoft Azure IoT SDK for Node.js repository you cloned from GitHub.

2.This terminal window is used as your device terminal. Go to the folder of your cloned repository, and navigate to the /azure-iot-sdk-node/device/samples/pnp folder.Install all the dependencies by running the following command:

npm install

3.Run the sample thermostat device with the following command:

node simple_thermostat.js

4.You see messages saying that the device has sent some information and reported itself online. These messages indicate that the device has begun sending telemetry data to the hub, and is now ready to receive commands and property updates. Don't close this terminal, you need it to confirm the service sample is working.

# Next Steps

Please refer to the below link for additional information for Plug and Play 

-   [Manage cloud device messaging with Azure-IoT-Explorer](https://github.com/Azure/azure-iot-explorer/releases)
-   [Import the Plug and Play model](https://docs.microsoft.com/en-us/azure/iot-pnp/concepts-model-repository)
-   [Configure to connect to IoT Hub](https://docs.microsoft.com/en-us/azure/iot-pnp/quickstart-connect-device-c)
-   [How to use IoT Explorer to interact with the device ](https://docs.microsoft.com/en-us/azure/iot-pnp/howto-use-iot-explorer#install-azure-iot-explorer)   
