---
platform: openwrt
device: mc-technologies mc100 gpio gateway
language: c/c++
---

Run a simple C sample on MC-Technologies MC100 GPIO Gateway running OpenWRT
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect MC-Technologies MC100 GPIO Gateway running OpenWRT with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Install [Docker](https://docs.docker.com/install) on your development system running Microsoft Windows, MacOS or Linux
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   [MC-Technologies MC100 GPIO Gateway](https://www.mc-technologies.net/en/product-groups/data-techniques/mobile-terminals-gateways/MC100-GPIO-4G-LTE-Gateway)

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Connect the MC100-GPIO to your network

<a name="Build"></a>
# Step 3: Build and Run the sample

## 3.1 Load the Dockerfile and settup you cross-compile environment

-   Checkout the [MC100 Azure IoT Dockerfile repository](https://github.com/LAB-LZX/MC100-Azure-IoT-SDK)

-   Run the following command to build the cross-compiler environment

        docker build -t imx6:latest . --network=host

<a name="Step-3-2-Build"></a>
## 3.2 Build the demo

-   Checkout the [MC100 GPIO Azure IoT Demo](https://github.com/LAB-LZX/MC100-GPIO-Azure-IoT-Demo)

-   Build the demo

        docker build -t imx6_mbyz:latest . --network=host

-   Copy the app to the local file system

        id=$(docker create imx6_mbyz)
        docker cp $id:/home/builder/src/cmake/mbyz ./mbyz-openwrt-imx6
        docker rm -v $id

-   Copy the app to your MC100-GPIO

        scp ./mbyz-openwrt-imx6 root@mc100.local.:.


## 3.3 Run the demo

-   Log in as root to you MC100 GPIO Gateway using Putty or SH

-   Start the app with the connectionstring as argument:

        ./mbyz-openwrt-imx6 <connectionstring>

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

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