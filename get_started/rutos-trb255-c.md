---
platform: rutos
device: trb255
language: c
---

Run a simple C sample on TRB255 device running RUTOS
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Run the Program](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect TRB255 device running RUTOS with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   TRB255 device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   [Login to device WebUI][rut-login]
-   Make sure to have internet connection
-   Download Azure IotHub package from Package Manager (Services->Package Manager)

 ![Alt text](./media/rutx12/Networking_rutx_configuration_examples_package_manager_v1.png)

-   Go to Azure IoTHub configuration page (Services->Cloud connections->Azure Iot Hub)
-   [More information can be found in our wiki page][wiki-page]

<a name="Build"></a>
# Step 3: Run the program

## Run the Sample

-   Check the "Enable Azure IoTHub monitoring" checkbox
-   Enter device connection string to "Connection string" placeholder
-   Enter interval in seconds to send data to "Message sending interval (sec.)" placeholder
-   Check all the checkboxes of information needed to send to cloud (in "GSM Values" placeholder)
-   Press "Save & Apply"

![Alt text](./media/rutx12/Networking_rutx_configuration_examples_azure_iot_hub_v1.png)

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects router modem data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

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
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[main-picture]: https://wiki.teltonika-networks.com/wikibase/images/f/ff/Networking_rutx_configuration_examples_azure_iot_hub_v1.png
[wiki-page]: https://wiki.teltonika-networks.com/view/Azure_IoT_Hub_cloud_connection
[rut-login]: https://wiki.teltonika-networks.com/view/RUTX08_First_Start#Login_to_device
[package-manager]: https://wiki.teltonika-networks.com/wikibase/images/1/10/Networking_rutx_configuration_examples_package_manager_v1.png
