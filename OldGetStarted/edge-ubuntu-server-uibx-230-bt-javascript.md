---
platform: edge ubuntu server
device: uibx-230-bt
language: javascript
---

Run a sample on uIBX-230-BT device running Ubuntu Server 16.04 
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Manual Test for Azure IoT Edge on device](#Manual)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect uIBX-230-BT device running Ubuntu Server 16.04 with Azure IoT Edge Runtime pre-installed and Device Management. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and Deploy client component to test device management capability 

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   [Sign up to IOT Hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Add the Edge Device](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux)
-   [Add the Edge Modules](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux#deploy-a-module)

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   [uIBX-230-BT device](https://www.ieiworld.com/en/product/model.php?II=268)

<a name="Manual"></a>
# Step 3: Manual Test for Azure IoT Edge on device

This section walks you through the test to be performed on the Edge devices running the Linux operating system such that it can qualify for Azure IoT Edge certification.

<a name="Step-3-1-IoTEdgeRunTime"></a>
## 3.1 Edge RuntimeEnabled 

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

 ![](./media/uibx-230-bt/1.jpg)

Open the command prompt on your IoT Edge device, confirm that the module deployed from the cloud is running on your IoT Edge device

    sudo iotedge list

 ![](./media/uibx-230-bt/2.jpg)

On the device details page of the Azure, you should see the runtime modules - edgeAgent, edgeHub and tempSensor modueles are under running status

 ![](./media/uibx-230-bt/3.jpg)

<a name="Step-3-2-DeviceManagement"></a>
## 3.2 Device Management

**Pre-requisites:** Device Connectivity.

**Description:** A device that can perform basic device management operations (Reboot and Firmware update) triggered by messages from IoT Hub.

## 3.2.1 Firmware Update (Using Microsoft SDK Samples):

To run the simulated device application, open a shell or command prompt window and navigate to the **iot-hub/Tutorials/FirmwareUpdate** folder in the Node.js project you downloaded. Then run the following commands:

    npm install
    node SimulatedDevice.js "{your device connection string}"

 ![](./media/uibx-230-bt/4.jpg)

To run the back-end application, open another shell or command prompt window. Then navigate to the **iot-hub/Tutorials/FirmwareUpdate** folder in the Node.js project you downloaded. Then run the following commands:

    npm install
    node ServiceClient.js "{your service connection string}"

 ![](./media/uibx-230-bt/5.jpg)

## 3.2.2 Reboot ([Using Microsoft SDK Samples](https://docs.microsoft.com/zh-tw/azure/iot-hub/iot-hub-node-node-device-management-get-started)):

Specify the path {{enter the path}} where the components are installed 

    npm install
    dmpatterns_getstarted_service.js  

To run the back-end application, open another shell or command prompt window.

 ![](./media/uibx-230-bt/6.jpg)

    npm install
    dmpatterns_getstarted_device.js                                 

IoT device client will get the message and report the status to the device twin. 
  
 ![](./media/uibx-230-bt/7.jpg)

  
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md