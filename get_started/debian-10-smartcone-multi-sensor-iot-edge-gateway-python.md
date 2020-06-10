---
platform: debian 10
device: smartcone multi-sensor iot edge gateway
language: python 3.7
---

Run a simple Python sample on the SmartCone Multi-Sensor IoT Edge Gateway running Debian 10 (Buster).
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

This document describes how to connect to the SmartCone Multi-Sensor IoT Edge Gateway device running Debian 10 (Buster) with Azure IoT Edge Runtime pre-installed and Device Management. This multi-step process includes:

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
-   SmartCone Multi-Sensor IoT Edge Gateway device.
-   SmartCone Power Supply

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

## 2.1 Initial Configuration

Our device is already pre-programmed with the Patriot software. You only need to boot the device and you would then be able to navigate to our management dashboard. Simply set your device in the correct subnet (10.0.0.0/24). The management interface is found at 10.0.0.5. From there you can configure the device using a GUI. 
If you wish to SSH directly into the device, you can use the following:

    # ssh smartcone@10.0.0.5

The default password is also `smartcone`.
You can confirm that our IoT engine is running by typing in:

    # docker ps

If nothing is found, restart the Patriot engine using the following command:

    # cd /opt/smartcone/patriot/
    # docker-compose up --build

## 2.2 Test Patriot Engine

Now that you know the engine is running, you can add a sample rule to check that the engine is responding to sensor input.

-   Create a Rule
-   Go to the front end dashboard, log in and click on Config on the left side
-   Then click on Rules
-   Set a simple rule from the drop-down menus.

There would be a demo sensor that would be on. In normal operation, you would receive a set of sensors with the SmartCone device depending on your configuration. Sensors would be pre-configured and already sending information to Patriot.

Using the SmartCone ecosystem is very simple and user friendly. No coding required.

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

  
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md