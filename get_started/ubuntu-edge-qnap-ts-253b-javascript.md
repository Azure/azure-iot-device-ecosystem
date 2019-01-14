---
platform: ubuntu sever
device: qnap ts-253b edge
language: javascript
---

Run a simple JavaScript sample on QNAP TS-253B device running Ubuntu Sever 16.04. 
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

This document describes how to connect QNAP TS-253B device running Ubuntu Sever 16.04 with Azure IoT Edge Runtime pre-installed and Device Management. This multi-step process includes:

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
-   QNAP TS-253B device.


<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Please refer to "Steps for x86 models – using Linux Station" session in [HOW TO INSTALL AND CONFIGURE AZURE IOT EDGE ON QNAP NAS](https://qiot.qnap.com/blog/en/2019/01/11/install-configure-azure-iot-edge-qnap-nas/) to install Azure IoT edge runtime on QNAP TS-253B device. 

<a name="Manual"></a>
# Step 3: Manual Test for Azure IoT Edge on device

This section walks you through the test to be performed on the Edge devices running the Linux operating system such that it can qualify for Azure IoT Edge certification.

<a name="Step-3-1-IoTEdgeRunTime"></a>
## 3.1 Edge RuntimeEnabled (Mandatory)

**Details of the requirement:**

The following components come pre-installed or at the point of distribution on the device to customer(s):

-   Azure IoT Edge Security Daemon
-   Daemon configuration file
-   A container management system
-   A version of `hsmlib` 

*Edge Runtime Enabled:*

**Check the iotedge daemon command:** 

Open the command prompt on your IoT Edge device , confirm that the Azure IoT edge Daemon is under running state

    systemctl status iotedge

 ![](./media/qnap/Capture.png)

Open the command prompt on your IoT Edge device, confirm that the module deployed from the cloud is running on your IoT Edge device

    sudo iotedge list

 ![](./media/qnap/iotedgedaemon.png) 

On the device details page of the Azure, you should see the runtime modules - edgeAgent, edgeHub and tempSensor modueles are under running status

 ![](./media/qnap/tempSensor.png)

<a name="Step-3-2-DeviceManagement"></a>
## 3.2 Device Management (Mandatory)

**Pre-requisites:** Device Connectivity.

**Description:** A device that can perform basic device management operations (Reboot and Firmware update) triggered by messages from IoT Hub.

## 3.2.1 Firmware Update (Using Microsoft SDK Samples):

Download the sample Node.js project from https://github.com/Azure-Samples/azure-iot-samples-node/archive/master.zip and extract the ZIP archive.

To run the simulated device application, open a shell or command prompt window and navigate to the **iot-hub/Tutorials/FirmwareUpdate** folder in the Node.js project you downloaded. Then run the following commands:

    npm install
    node SimulatedDevice.js "{your device connection string}"

To run the back-end application, open another shell or command prompt window. Then navigate to the **iot-hub/Tutorials/FirmwareUpdate** folder in the Node.js project you downloaded. Then run the following commands:

    npm install
    node ServiceClient.js "{your service connection string}"

IoT device client will get the message and report the status to the device twin.

 ![](./media/qnap/devicetwin.png)


## 3.2.2 Reboot (Using Microsoft SDK Samples):

Create a simulated device app that contains a direct method that reboots that device. Direct methods are invoked from the cloud - 
[Create a simulated device app](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-node-node-device-management-get-started#create-a-simulated-device-app)

Create a Node.js console app that calls the reboot direct method in the simulated device app through your IoT hub -
[Trigger a remote reboot on the device using a direct method](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-node-node-device-management-get-started#trigger-a-remote-reboot-on-the-device-using-a-direct-method)

After the completion of the previous 2 steps, you have two Node.js console apps:

**dmpatterns_getstarted_device.js**, which connects to your IoT hub with the device identity created earlier, receives a reboot direct method, simulates a physical reboot, and reports the time for the last reboot.

**dmpatterns_getstarted_service.js**, which calls a direct method in the simulated device app, displays the response, and displays the updated reported properties.

At the command prompt in the manageddevice folder, run the following command to begin listening for the reboot direct method. 

    node dmpatterns_getstarted_device.js 


At the command prompt in the triggerrebootondevice folder, run the following command to trigger the remote reboot and query for the device twin to find the last reboot time. 

    node dmpatterns_getstarted_service.js 


You see the device response to the direct method and report the status to the device twin in the console

 ![](./media/qnap/DeviceReboot.png)

<a name="NextSteps"></a>
# Step 4: Next steps

You have now learned how to invoke a direct method from a back-end app . To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:
 
-   [Manage cloud device messaging with iothub-explorer]
-   [Save IoT Hub messages to Azure data storage]
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub]
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]
-   [Remote monitoring and notifications with Logic Apps]   
 
[Manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[Save IoT Hub messages to Azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[Use Power BI to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[Remote monitoring and notifications with Logic Apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[setup-devbox-python]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/get_started/python-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

<a name="Step-5-Troubleshooting"></a>
# Step 5: Troubleshooting

Please contact engineering support on [helpdesk.qnap.com](https://helpdesk.qnap.com) for help with troubleshooting.
  
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md