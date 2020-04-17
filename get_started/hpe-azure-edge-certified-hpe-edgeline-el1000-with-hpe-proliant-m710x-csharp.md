---
platform: hpe-azure-edge-certified
device: hpe edgeline el1000 with hpe proliant m710x
language: csharp
---

Create and configure an Azure IoT Edge device using an HPE EL1000 with HPE ProLiant m710x running an Azure IoT Edge supported [Tier 1 OS][lnk-edge-support]
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

This document describes how to configure an HPE Edgeline EL1000 with HPE ProLiant m710x running an Azure IoT Edge supported [Tier 1 OS][lnk-edge-support] with the Azure IoT Edge Runtime. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Installing the Azure IoT Edge runtime

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup the IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-register-device-portal)
-   HPE Edgeline EL1000 with HPE ProLiant m710x device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Follow the instructions in the [Operating System Deployment on HPE ProLiant Moonshot Server Cartridges User Guide][os-setup] to install the Operating System onto the device.

<a name="Manual"></a>
# Step 3: Manual Test for Azure IoT Edge on device

This section walks you through installing, configuring, and verifying Azure IoT Edge runtime on Linux.

-   [Prepare device for IoT Edge runtime installation](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#register-microsoft-key-and-software-repository-feed)
-   [Install the container runtime](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#install-the-container-runtime)
-   [Install the Azure IoT Edge runtime](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#install-the-azure-iot-edge-security-daemon)
-   [Configure the Azure IoT Edge runtime](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#configure-the-azure-iot-edge-security-daemon)
-   [Verify success](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#verify-successful-installation)

<a name="NextSteps"></a>
# Next Steps

You have now learned how to create and configure an Azure IoT Edge device. To explore how to deploy Azure IoT Edge modules, and how to store, analyze and visualize the data in Azure using a variety of different services, please click on the following lessons:

-   [Deploy Azure IoT Edge modules from the Azure portal](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-deploy-modules-portal)
-   [Manage cloud device messaging with iothub-explorer](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
-   [Save IoT Hub messages to Azure data storage](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
-   [Weather forecast using the sensor data from an IoT hub in Azure Machine Learning](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
-   [Remote monitoring and notifications with Logic Apps](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)


[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[os-setup]: https://support.hpe.com/hpsc/doc/public/display?docId=c03933547
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[lnk-edge-support]: https://docs.microsoft.com/en-us/azure/iot-edge/support#operating-systems




