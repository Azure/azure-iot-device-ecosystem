Run Azure IoT Edge Runtime on Express5800 T110j-S device running Ubuntu 18.04
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Manual Test for Azure IoT Edge on device](#Manual)
-   [Step 4: Additional Links](#AdditionalLinks)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect Express5800 T110j-S device running Ubuntu 18.04 with Azure IoT Edge Runtime pre-installed and Device Management. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and Deploy client component to test device management capability 

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Create an Azure account](https://azure.microsoft.com/en-us/free/)
-   [Sign up to Azure Portal](https://portal.azure.com/#home)
-   [Setup your IoT hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md)
-   [Add the Edge Device](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux)
-   [Add the Edge Modules](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux?view=iotedge-2018-06#deploy-a-module)
-   Express5800 T110j-S device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   [Express5800 T110j-S device](https://jpn.nec.com/pcserver/tower/t110js/index.html)

<a name="Manual"></a>
# Step 3: Manual Test for Azure IoT Edge on device

This section walks you through the test to be performed on the Edge devices running the Linux operating system such that it can qualify for Azure IoT Edge certification.

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

 ![](./images/Capture.png)

Open the command prompt on your IoT Edge device, confirm that the module deployed from the cloud is running on your IoT Edge device

    sudo iotedge list

 ![](./images/iotedgedaemon.png) 

On the device details page of the Azure, you should see the runtime modules - edgeAgent, edgeHub and tempSensor modueles are under running status

 ![](./images/tempSensor.png)

# Step 4: Additional Links

-   [What is Azure IoT Edge](https://docs.microsoft.com/en-us/azure/iot-edge/about-iot-edge?view=iotedge-2018-06)
-   [Azure IoT Edge 1.0.10 release is now available](https://azure.microsoft.com/en-us/updates/iot-edge1-0-10/)
-   [Azure IoT Edge supported systems](https://docs.microsoft.com/en-us/azure/iot-edge/support?view=iotedge-2018-06)
-   [Develop your own IoT Edge modules](https://docs.microsoft.com/en-us/azure/iot-edge/module-development?view=iotedge-2018-06)

