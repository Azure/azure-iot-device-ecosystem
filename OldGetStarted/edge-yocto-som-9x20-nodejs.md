---
platform: edge yocto 2.0
device: som-9x20
language: node.js
---

Run a simple Node.js sample on SOM-9X20 device running Yocto 2.0 (ARM32)
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

This document describes how to connect SOM-9X20 device running Yocto 2.0 (ARM32) with Azure IoT Edge Runtime pre-installed and Device Management. This multi-step process includes:

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
-   SOM-9X20 device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   **[Prepare SOM-9X20 image]**
-   Run Linux OS (Ubuntu 14.04) on development platform.
-   Download SOM-9X20 Yocto image from [here](https://www.viatech.com/en/software-downloads/)
    -   If you have not registry before, please join and create a account for you.
    -   Then entry your account and password to login.
    -   Select Yocto Azure IoT Edge EVK v0.1 label,and click "Agree" to download image.
    -   The final step, check your board, and ENTER YOUR SERIAL NUMBER TO DOWNLOAD.
-   Copy VT6093\_Yocto2.0\_EVK\_v0.1\_IoTEdge\_190529.zip to development platform and unzip files.
-   Plug micro USB to SOM-9X20 and connect with development platform
-   Press and hold SOM-9X20 "VOL\_DOWN" button and press "PW\_ON" button to enter the fastboot mode
-   Run command: apt-get install android-tools-fastboot  to download fastboot
-   Change folder to VT6093\_Yocto2.0\_EVK\_v0.1\_IoTEdge\_190529
-   Run command: ./fastboot\_bspinst\_ufs.sh
-   Wait firmware update and system will reboot.

-   **[Prepare install IoT Edge runtime]**
-   Change to /opt/azure_iotedge_runtime folder
-   Run command `./check_config.sh`, make sure all Generally Necessary section are display green
-   Run command: `./install.sh`
    -   Type "**32**" to install 32bit IoT edge runtime
    -   Type "**moby**" to install docker runtime
    -   Type "**n**", don't install SysV environment

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

**Check docker and iotedge daemon is running:** 

Open the command prompt on your IoT Edge device , restart the Azure Docker and IoT edge Daemon running state

    systemctl restart docker
    systemctl restart iotedge

**Check the iotedge daemon command:** 

Open the command prompt on your IoT Edge device , confirm that the Azure IoT edge Daemon is under running state

    systemctl status iotedge


 ![](./media/ArtiGO/Capture.png)

Open the command prompt on your IoT Edge device, confirm that the module deployed from the cloud is running on your IoT Edge device

    sudo iotedge list

 ![](./media/ArtiGO/iotedgedaemon.png) 

On the device details page of the Azure, you should see the runtime modules - edgeAgent, edgeHub and tempSensor modueles are under running status

 ![](./media/ArtiGO/tempSensor.png)

<a name="Step-3-2-DeviceManagement"></a>
## 3.2 Device Management (Optional)

**Pre-requisites:** Device Connectivity.

**Description:** A device that can perform basic device management operations (Reboot and Firmware update) triggered by messages from IoT Hub.

## 3.2.1 Firmware Update (Using Microsoft SDK Samples):

Specify the path {{enter the path}} where the firmwareupdate client components are installed.

To run the simulated device application, open a shell or command prompt window and navigate to the **iot-hub/Tutorials/FirmwareUpdate** folder in the Node.js project you downloaded. Then run the following commands:

    node SimulatedDevice.js "{your device connection string}"

To run the back-end application, open another shell or command prompt window. Then navigate to the **iot-hub/Tutorials/FirmwareUpdate** folder in the Node.js project you downloaded. Then run the following commands:

    node ServiceClient.js "{your service connection string}"

IoT device client will get the message and report the status to the device twin.

 ![](./media/ArtiGO/devicetwin.png)

**Update firmware**

Confirm the IoT hub, Device ID, method name and method payload as below:

-   Press “call Method” button
-   Check the returning status as below:

 ![](./media/ArtiGO/firmware.png)


## 3.2.2 Reboot (Using Microsoft SDK Samples):

Specify the path {{enter the path}} where the components are installed 

Confirm the IoT hub, Device ID, method name as below:

-   Press “call Method” button
-   Check the returning status as below:

 ![](./media/ArtiGO/reboot.png)


IoT device client will get the message and report the status to the device twin.

 ![](./media/ArtiGO/devicetwinmessage.png)
  
## 3.3.3 Firmware Update (Modified SDK samples/Custom made application):

If the Client components are custom made please add the steps to execute the Firmware Update through Device Twin.

**Note**: Client Components must be shipped with the device 

## 3.3.4 Reboot (Modified SDK samples/Custom made application):

If the Client components are custom made please add the steps to execute the Device Reboot through Direct Methods

**Note**: Client Components must be shipped with the device 
  
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md