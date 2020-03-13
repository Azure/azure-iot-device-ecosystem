---
platform: r2os ubuntu 16.04.6 lts
device: ra340
language: neutral
---

Run a simple sample on RA340 device running R2OS MASE Ubuntu Server 16.04.6
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Manual Test for Azure IoT Edge on device](#Manual)
-   [Step 4: Next Steps](#NextSteps)
-   [Step 5: Troubleshooting](#Step-5-Troubleshooting)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect RA340 device running R2OS MASE Ubuntu Server 16.04.6 with Azure IoT Edge Runtime pre-installed and Device Management. This multi-step process includes:

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
-   Relay2 RA340 device.
-   Relay2 Cloud Management account (AP Customer Account)

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

Please login to the Relay2 Cloud Management system (<https://www.relay2.net>) with your AP Customer Account credentials and navigate to your App Store (App Manager -> App Store from the  main menu bar). Select the **Azure IoT Edge** app container from the list of applications and click on the associated "Get" button to move the app to your accounts **My Apps** section.

Navigate to the **My Apps** section by using **App Manager -> My Apps** from the main menu bar. Click on the "Azure IoT Edge" app to open the application specific configuration page {TODO add screenshot}. 

Select the **Profile** tab and copy your IoT Edge Hub device connection string to the correcponding input box of the selected profile. Click the **Save** button to save the connection string and close the profile popup window.

Click on the **Install** tab and select the mobiliy domain of your RA340 device and select the previously updated profile name from the drop down list. Click the **Apply** button to install the IoT Edge Container to the selected RA340 device(s).
Now your RA340 device is ready and should connect to the configured IoT Edge Hub automatically.

<a name="Manual"></a>
# Step 3: Manual Test for Azure IoT Edge on device

This section walks you through the test to be performed on the Edge devices running the Linux operating system such that it can qualify for Azure IoT Edge certification.

<a name="Step-3-1-IoTEdgeRunTime"></a>
## 3.1 Edge RuntimeEnabled (Mandatory)

**Details of the requirement:**

The following components are installed automatically once you deploy the "Azure IoT Edge" container application from the Relay2 App Store:

-   Azure IoT Edge Security Daemon
-   Daemon configuration file
-   Moby container management system
-   A version of `hsmlib` 

*Edge Runtime Enabled:*

**Check the iotedge daemon command:** 

Open the command prompt on your IoT Edge device , confirm that the Azure IoT edge Daemon is under running state

    $ /etc/init.d/iotedge status
    * iotedge is running
    $

Open the command prompt on your IoT Edge device, confirm that the module deployed from the cloud is running on your IoT Edge device

    $ iotedge list 

    $ iotedge list
    NAME               STATUS           DESCRIPTION      CONFIG
    edgeAgent          running          Up 13 hours      mcr.microsoft.com/azureiotedge-agent:1.0
    edgeHub            running          Up 4 minutes     mcr.microsoft.com/azureiotedge-hub:1.0
    TestIoTEdgeModule  running          Up 4 minutes     mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0
    $

On the device details page of the Azure, you should see the runtime modules - edgeAgent, edgeHub and tempSensor modules are under running status

 ![](./media/ArtiGO/tempSensor.png)

<a name="Step-3-2-DeviceManagement"></a>
## 3.2 Device Management (Optional)

Device Management and Firmware upgrade is not supported through Azure IoT Edge - please use the corresponding functions of the Relay2 Cloud Management System for this. For details please refer to the Relay2 AP Customer User Guide {{TODO Link to the manual PDF}}

## 3.2.2 Reboot (Using Microsoft SDK Samples):

To reboot the RA340 device, please use the Relay2 Cloud Management System and navigate to the **Configure -> Access Point** page and select your specific device which you want to reboot from the list of devices assigned to you account. In the device specific configuration page please click the **Reboot** button in the top right corner and click the **Confirm** button in the Reboot popup window.



