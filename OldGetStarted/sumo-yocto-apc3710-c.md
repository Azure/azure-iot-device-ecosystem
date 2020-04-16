---
platform: sumo yocto 2.5
device: apc3710
language: c
---

Create and configure an Azure IoT Edge device using an APC3710 running an Azure IoT Edge supported [Tier 1 OS](https://docs.microsoft.com/en-us/azure/iot-edge/support)
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

This document describes how to connect APC3710 device running Sumo Yocto 2.5 with Azure IoT Edge Runtime pre-installed and Device Management. This multi-step process includes:

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
-   APC3710 device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Follow the instructions in the Operating System Deployment on APC3710 User Guide to install the Operating System onto the device.(<https://pan.wps.cn/l/sK8DnHNeI>)

<a name="Manual"></a>
# Step 3: Manual Test for Azure IoT Edge on device

This section walks you through the test to be performed on the Edge devices running the Linux operating system such that it can qualify for Azure IoT Edge certification.

-   [Prepare device for IoT Edge runtime installation](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#register-microsoft-key-and-software-repository-feed)

-   [Install the container runtime](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#install-the-container-runtime)

-   [Install the Azure IoT Edge runtime](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#install-the-azure-iot-edge-security-daemone)

-   [Configure the Azure IoT Edge runtime](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#configure-the-azure-iot-edge-security-daemon)

-   [Verify success](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#verify-successful-installation)

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

 ![](./media/ArtiGO/Capture.png)

Open the command prompt on your IoT Edge device, confirm that the module deployed from the cloud is running on your IoT Edge device

    sudo iotedge list

 ![](./media/ArtiGO/iotedgedaemon.png) 

On the device details page of the Azure, you should see the runtime modules - edgeAgent, edgeHub and tempSensor modueles are under running status

 ![](./media/ArtiGO/tempSensor.png)


[Deploy Azure IoT Edge modules from the Azure portal](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-deploy-modules-portal)

[Manage cloud device messaging with iothub-explorer](https://docs.microsoft.com/zh-cn/azure/iot-hub/iot-hub-vscode-iot-toolkit-cloud-device-messaging)

[Save IoT Hub messages to Azure data storage](https://docs.microsoft.com/zh-cn/azure/iot-hub/tutorial-routing#routing-to-a-storage-account)

[Use Power BI to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)

[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)

[Weather forecast using the sensor data from an IoT hub in Azure Machine Learning](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning)

[Remote monitoring and notifications with Logic Apps](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)

  
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md