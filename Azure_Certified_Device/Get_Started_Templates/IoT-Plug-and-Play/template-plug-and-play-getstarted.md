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
- Step 3: Prepare the environment
- Step 4: Build and Run the Sample
- Additional Links

<a name="Introduction"></a>

# Introduction 

**About this document**

This document describes how to connect [HBFBU691-3455-B](https://www.jetwayipc.com/product/hbfbu691-3455-b-series/) device running Windows 10 with Azure IoT SDK. This multi-step process includes:

  IoT Plug and Play certified device simplifies the process of building devices without custom device
  code. Using Solution builders can integrated quickly using the certified IoT Plug and Play enabled
  device based on Azure IoT Central as well as third-party solutions.
  
  This getting started guide provides step by step instruction on getting the device provisioned to Azure
  IoT Hub using Device Provisioning Service (DPS) and using Azure IoT Explorer to interact with
  device's capabilities.

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:
- [Azure Account](https://portal.azure.com/) 
- [Azure IoT Hub Device Provisioning Service](https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision)
- [Setup your IoT hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md)
- [Provision your device and get its credentials](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md)


<a name="preparethedevice"></a>
# Step 2: Prepare the Device
- HBFBU691-3455-B device.
- Make sure the Internet is on the machine, and then execute the azure-iot connection Azure

# Step 3: Prepare the environment

- Installed Windows 10 on your machine.
- Installed [Node.js v4.0+](https://nodejs.org/) on your machine.
- Installed [Git](https://git-scm.com/download/) on your machine
- Create a new DPS using the [Azure Portal](https://portal.azure.com/) 
- Create an Azure IoThub using the [Azure Portal](https://portal.azure.com/) 
- Register a new device in the IoT hub using the [Azure Portal](https://portal.azure.com/) 


# Step 4: Build and Run the sample

**4.1 Build SDK and sample**

- Download latest SDK using following command:

  git clone https://github.com/Azure/azure-iot-sdk-node

- Install required libraries
  In a local terminal window, go to the folder of your cloned repository and navigate to the /azure-iot-sdk-node/device/samples/pnp folder. Then run the following command to       install the required libraries:

  npm install

- Open the file simple_thermostat.js in a text editor.
- Locate the following code in the file:

  let connectionString = "[device connection string]"
  
- Replace [device connection string] with the connection string for your device. Save the changes.
   
   //DPS connection information
   
   const provisioningHost = "global.azure-devices-provisioning.net"
   
   const idScope = "[YouridScope]"
   
   const registrationId = "[YourregistrationId]"
   
   const symmetricKey = "[YoursymmetricKey]"
   
   const useDps = "DPS"
  
- Replace [DPS connection information] with the string for your device. Save the changes.
  
**4.2 Send Device Events to IoT Hub**

- Run the sample thermostat device with the following command:

  node simple_thermostat.js

**4.3 Integration with Azure IoT Explorer**
  Connect
  
- Enter the connection string from Azure CLI and press Save. You will see a device symm-key-device
  
  az iot hub show-connection-string --name my-sample-hub --key primary --query connectionString -o tsv
  [pic1]
  
  Telemetry
- Press the device name and choose the Telemetry Page. Press Start and wait a momnent. You will
  see the telemetry data on the screen.
  [pic2]
  Property and Command
- You can press the item in the green rectangle to show the properties.
- You can press the item in the blue rectangle to start a command.
  [pic3]
  
  

# Additional Links

  Please refer to the below link for additional information for Plug and Play 

-   [Manage cloud device messaging with Azure-IoT-Explorer](https://github.com/Azure/azure-iot-explorer/releases)
-   [Import the Plug and Play model](https://docs.microsoft.com/en-us/azure/iot-pnp/concepts-model-repository)
-   [Configure to connect to IoT Hub](https://docs.microsoft.com/en-us/azure/iot-pnp/quickstart-connect-device-c)
-   [How to use IoT Explorer to interact with the device ](https://docs.microsoft.com/en-us/azure/iot-pnp/howto-use-iot-explorer#install-azure-iot-explorer)   
