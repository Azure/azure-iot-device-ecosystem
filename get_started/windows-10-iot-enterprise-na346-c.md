---
platform: windows10 iot enterprise
device: na346
language: c
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Manual Test for Azure IoT Edge on device](#Manual)
-   [Step 4: Troubleshooting](#Step-5-Troubleshooting)

<a name="Introduction"></a>
# Introduction

**About this document**

-   This document describes how to connect NA346 device running Windows10 IoT and how to setup Azure IoT Edge Runtime. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Configuring Azure IoT Edge Runtime
-   Build and Deploy client component to test device management capability 

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Sign up to Azure](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Create the IoT Hub, Register the device, Install the Runtime, and Deploy the Edge module](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart)
-   Axiomtek NA346 device,keyboard,mouse,and monitor

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   You prepare the device on your hand, and connect keyboard, mouse, and monitor to device. after that You just login Windows and perform next steps.

<a name="Manual"></a>
# Step 3: Manual Test for Azure IoT Edge on device

This section walks you through the test to be performed on the Edge devices running the Windows operating system such that it can qualify for Azure IoT Edge certification.

<a name="Step-3-1-IoTEdgeRunTime"></a>
## 3.1 Edge RuntimeEnabled (Mandatory)

**Details of the requirement:**

before makeing sure IoT Edge services and modules running, You should perform prerequisite steps. 
its instruction is pointed out in Step 1.

*Edge Runtime Enabled:*

Open the powershell on your IoT Edge device , confirm the status of the IoT Edge service.

    Get-Service iotedge

Examine service logs from the last 5 minutes. If you just finished installing the IoT Edge runtime, you may see a list of errors from the time between running **Deploy-IoTEdge** and **Initialize-IoTEdge**. These errors are expected, as the service is trying to start before being configured. 

    . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog

List running modules. After a new installation, the only module you should see running is **edgeAgent**. After you [deploy IoT Edge modules](how-to-deploy-modules-portal.md), you will see others. 

    iotedge list

![](./media/Axiomtek/edgemodule_status.png)

View the messages being sent from the module you created to the cloud.

    iotedge logs {module name}

![](./media/Axiomtek/edgemodule_logs.png)


<a name="Step-5-Troubleshooting"></a>
# Step 4: Troubleshooting

Please contact engineering support on **<mailto:tracy.ku@axiomtek.com.tw>** for help with troubleshooting.
  
[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md
[lnk-manage-iot-hub]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md
