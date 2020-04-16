---
platform: windows 10 iot enterprise
device: eivp1300
language: c
---

Run a simple C sample on eIVP1300 device running Windows 10 IoT Enterprise
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

This document describes how to connect eIVP1300 device running Windows 10 IoT Enterprise with Azure IoT Edge Runtime pre-installed and Device Management. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and Deploy client component to test device management capability 

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-windows]
-   [Setup your IoT hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   [Sign up to IOT Hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Add the Edge Device](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart)
-   [Add the Edge Modules](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart#deploy-a-module)
-   eIVP1300 device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Connect eIVP1300 to a monitor with HDMI cable.
-   Connect eIVP1300 to the power source.
-   Power on the eIVP1300 after hardware connection.

<a name="Manual"></a>
# Step 3: Manual Test for Azure IoT Edge on device

This section walks you through the test to be performed on the Edge devices running the Windows operating system such that it can qualify for Azure IoT Edge certification.

<a name="Step-3-1-IoTEdgeRunTime"></a>
## 3.1 Edge RuntimeEnabled (Mandatory)

**Details of the requirement:**

The following components come pre-installed or at the point of distribution on the device to customer(s):

-   Azure IoT Edge Security Daemon
-   Daemon configuration file
-   Moby container management system
-   A version of `hsmlib` 

*Edge Runtime Enabled:*

Open the powershell on your IoT Edge device , confirm the status of the IoT Edge service.

    Get-Service iotedge

Examine service logs from the last 5 minutes. If you just finished installing the IoT Edge runtime, you may see a list of errors from the time between running **Deploy-IoTEdge** and **Initialize-IoTEdge**. These errors are expected, as the service is trying to start before being configured. 

    . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog

List running modules. After a new installation, the only module you should see running is **edgeAgent**. After you [deploy IoT Edge modules](how-to-deploy-modules-portal.md), you will see others. 

    iotedge list

![](./media/windows_edge_images/edgemodule_status.png)

View the messages being sent from the module you created to the cloud.

    iotedge logs {module name}

![](./media/windows_edge_images/edgemodule_logs.png)

[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
