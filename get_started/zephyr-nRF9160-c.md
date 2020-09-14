---
platform: Zephyr
device: Nordic Semiconductor nRF9160
language: c
---

# Run a simple C sample on Nordic Semiconductor's nRF9160 running Zephyr

---

# Table of Contents

- [Introduction](#Introduction)
- [Step 1: Prerequisites](#Prerequisites)
- [Step 2: Build and Run the Sample](#Build)
- [Next Steps](#NextSteps)

<a name="Introduction"></a>

# Introduction

**About this document**

This document describes how to connect Nordic Semiconductor's nRF9160 running Zephyr with the Azure IoT Hub sample application. This multi-step process includes:

- Configuring Azure IoT Hub
- Registering your IoT device
- Build and deploy the Azure IoT Hub sample application on device

<a name="Prerequisites"></a>

# Step 1: Prerequisites

You should have the following items ready before beginning the process:

- [Prepare your development environment][setup-devbox-linux]
- [Setup your IoT hub][lnk-setup-iot-hub]
- [Provision your device and get its credentials][lnk-manage-iot-hub]
- a nRF9160 Development Kit, for example the [Thingy:91](https://www.nordicsemi.com/Software-and-tools/Prototyping-platforms/Nordic-Thingy-91) or the [nRF9160 DK](https://www.nordicsemi.com/Software-and-tools/Development-Kits/nRF9160-DK)
- the [nRF Connect SDK](https://www.nordicsemi.com/Software-and-tools/Software/nRF-Connect-SDK) (see [Getting started](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/getting_started.html))

<a name="Build"></a>

# Step 2: Build and Run the sample

[The documentation for the Azure IoT Hub sample application](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/samples/nrf9160/azure_iot_hub/README.html) in the nRF Connect SDK explains how to build and run the sample.

<a name="NextSteps"></a>

# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

- [Manage cloud device messaging with iothub-explorer]
- [Save IoT Hub messages to Azure data storage]
- [Use Power BI to visualize real-time sensor data from Azure IoT Hub]
- [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
- [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]
- [Remote monitoring and notifications with Logic Apps]

[manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[save iot hub messages to azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[use power bi to visualize real-time sensor data from azure iot hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[use azure web apps to visualize real-time sensor data from azure iot hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[weather forecast using the sensor data from your iot hub in azure machine learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[remote monitoring and notifications with logic apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../../setup_iothub.md
[lnk-manage-iot-hub]: ../../manage_iot_hub.md
