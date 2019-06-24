---
platform: {Sumo Yocto 2.5}
device: {APC3710}
language: {English}
---



*We highly recommend keeping this document current, and Microsoft reserves a right to remove devices and documents from the Azure IoT Device Catalog if document contains broken URL links, incorrect information etc.*

Run a simple {enter the language used to you edge device} sample on {enter your device name here} device running {enter the OS name running on edge device. Specify distribution or Windows SKU information. Ex: Ubuntu Sever 16.04, Windows 10 IoT Core. Only [Tier 1 OS](https://docs.microsoft.com/en-us/azure/iot-edge/support) is allowed}
===
---

Create and configure an Azure IoT Edge device using an APC3710 running an Azure IoT Edge supported Tier 1 OS


# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Manual Test for Azure IoT Edge on device](#Manual)
-   [Step 4: Next Steps](#NextSteps)
-   [Step 5: Troubleshooting](#Step-5-Troubleshooting)

# Instructions for using this template

-   Replace the text in {placeholders} with correct values.
-   Delete the lines {{enclosed}} after following the instructions enclosed between them.
-   It is advisable to use external links, wherever possible.
-   Remove this section from final document.

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect {enter your device name here} device running {enter the OS name running on edge device} with Azure IoT Edge Runtime pre-installed and Device Management. This multi-step process includes:

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
-   {enter your device name here} device.
-   {{Please specify if any other software(s) or hardware(s) are required.}}

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   {Follow the instructions in the Operating System Deployment on APC3710 User Guide to install the Operating System onto the device.(https://pan.wps.cn/l/sK8DnHNeI)}

<a name="Manual"></a>
# Step 3: Manual Test for Azure IoT Edge on device

This section walks you through the test to be performed on the Edge devices running the Linux operating system such that it can qualify for Azure IoT Edge certification.

[Prepare device for IoT Edge runtime installation](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#register-microsoft-key-and-software-repository-feed)

[Install the container runtime](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#install-the-container-runtime)

[Install the Azure IoT Edge runtime](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#install-the-azure-iot-edge-security-daemone)

[Configure the Azure IoT Edge runtime](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#configure-the-azure-iot-edge-security-daemon)

[Verify success](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#verify-successful-installation)

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

<a name="Step-3-2-DeviceManagement"></a>
## 3.2 Device Management (Optional)

**Pre-requisites:** Device Connectivity.

**Description:** A device that can perform basic device management operations (Reboot and Firmware update) triggered by messages from IoT Hub.

## 3.2.1 Firmware Update (Using Microsoft SDK Samples):

Specify the path {{enter the path}} where the firmwareupdate client components are installed.

To run the simulated device application, open a shell or command prompt window and navigate to the **iot-hub/Tutorials/FirmwareUpdate** folder in the Node.js project you downloaded. Then run the following commands:

    npm install
    node SimulatedDevice.js "{your device connection string}"

To run the back-end application, open another shell or command prompt window. Then navigate to the **iot-hub/Tutorials/FirmwareUpdate** folder in the Node.js project you downloaded. Then run the following commands:

    npm install
    node ServiceClient.js "{your service connection string}"

IoT device client will get the message and report the status to the device twin.

 ![](./images/devicetwin.png)

**Update firmware**

Confirm the IoT hub, Device ID, method name and method payload as below:

-   Press “call Method” button
-   Check the returning status as below:

 ![](./images/firmware.png)


## 3.2.2 Reboot (Using Microsoft SDK Samples):

Specify the path {{enter the path}} where the components are installed 

Confirm the IoT hub, Device ID, method name as below:

-   Press “call Method” button
-   Check the returning status as below:

 ![](./images/reboot.png)


IoT device client will get the message and report the status to the device twin.

 ![](./images/devicetwinmessage.png)
  
## 3.3.3 Firmware Update (Modified SDK samples/Custom made application):

If the Client components are custom made please add the steps to execute the Firmware Update through Device Twin.

**Note**: Client Components must be shipped with the device 

## 3.3.4 Reboot (Modified SDK samples/Custom made application):

If the Client components are custom made please add the steps to execute the Device Reboot through Direct Methods

**Note**: Client Components must be shipped with the device 

<a name="NextSteps"></a>
# Step 4: Next steps

Once you shared the documents with us, we will contact you in the following 48 to 72 business hours with next steps.

[Deploy Azure IoT Edge modules from the Azure portal](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-deploy-modules-portal)

[Manage cloud device messaging with iothub-explorer](https://docs.microsoft.com/zh-cn/azure/iot-hub/iot-hub-vscode-iot-toolkit-cloud-device-messaging)

[Save IoT Hub messages to Azure data storage](https://docs.microsoft.com/zh-cn/azure/iot-hub/tutorial-routing#routing-to-a-storage-account)

[Use Power BI to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)

[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)

[Weather forecast using the sensor data from an IoT hub in Azure Machine Learning](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning)

[Remote monitoring and notifications with Logic Apps](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)


<a name="Step-5-Troubleshooting"></a>
# Step 5: Troubleshooting

Please contact engineering support on **<mailto:iotcert@microsoft.com>** for help with troubleshooting.
  
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
