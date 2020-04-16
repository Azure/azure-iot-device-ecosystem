---
platform: edge debian 9
device: uc-8100a-me-t series
language: c
---

Run a simple C sample on UC-8100A-ME-T series device running Debian 9
===
---

# Table of Contents

- [Introduction](#Introduction)
- [Step 1: Prerequisites](#Prerequisites)
- [Step 2: Prepare your Device](#PrepareDevice)
- [Step 3: Manual Test for Azure IoT Edge on device](#Manual)

<a name="Introduction"></a>

# Introduction

**About this document**

This document describes how to connect UC-8100A-ME-T device running Debian 9 with Azure IoT Edge Runtime pre-installed. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and Deploy client component to test device management capability 

<a name="Prerequisites"></a>

# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Sign up to IOT Hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Add the Edge Device](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux)
-   [Add the Edge Modules](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux#deploy-a-module)
-   UC-8100A-ME-T series device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   UC-8100A-ME-T series device with Debian 9 OS and pre-installed Azure IoT Edge Runtime.


<a name="Manual"></a>

# Step 3: Manual Test for Azure IoT Edge on device

<a name="Step-3-1-IoTEdgeRunTime"></a>

## 3.1 Edge RuntimeEnabled (Mandatory)

**Details of the requirement:**

The following components come pre-installed or at the point of distribution on the device to customer(s):

-   Azure IoT Edge Security Daemon
-   Daemon configuration file
-   Moby container management system
-   A version of `hsmlib` 

*Edge Runtime Enabled:*

**Check the iotedge daemon command:** 

Open the command prompt on your IoT Edge device , confirm that the Azure IoT edge Daemon is under running state

    systemctl status iotedge

 ![](./media/moxa/Capture.png)

Open the command prompt on your IoT Edge device, confirm that the module deployed from the cloud is running on your IoT Edge device

    sudo iotedge list

 ![](./media/moxa/iotedgedaemon.png) 

On the device details page of the Azure, you should see the runtime modules - edgeAgent, edgeHub and tempSensor modueles are under running status

 ![](./media/moxa/tempSensor.png)

[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
 
