---
platform: linux (msc-ldk)
device: baytrail
language: c
---

Run a simple C sample on a Baytrail family device running MSC-LDK
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Tips](#tips)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect the Baytrail device running MSC-LDK with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Baytrail module and baseboard.
-   MSC-LDK (Linux Development Kit)

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-   Follow the instructions in chapter "Getting Started" of the [devices user manual][lnk-user-manual]

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

-   The Microsoft Azure IoT Device SDK for C is already installed in the standard images as the package "azure-iot-sdk-c-samples".

-   These images can be built after MSC-LDK installation with:

        make msc-image-sato
    
    or
    
        make msc-image-lxqt

## 3.2 Send Device Events to IoT Hub:

-   Open a telnet session and connect to the device.
-   Run the sample by issuing following command:

    **For HTTP:**

        iothub_client_sample_http "<device connection string>"
        
-   Replace the above placeholder "&lt;device connection string&gt;" with device connection string you obtained in [Step 1](#Step-1:-Prerequisites) and save the changes.

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

<a name="tips"></a>
# Tips

- If you just want to build the serializer samples, run the following commands:

        ./build.sh bitbake -c cleanall azure-iot-sdk-c
        ./build.sh bitbake -c compile azure-iot-sdk-c
        ./build.sh bitbake -c devshell azure-iot-sdk-c

    and in the devshell
  
        ../temp/run.do_compile

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
[lnk-user-manual]: http://www.msc-technologies.eu/fileadmin/documentpool/Support-Center/Qseven/Q7-BT/20-Drivers/80-Linux/MSC_Q7-BT-ldk-bsp-C984.pdf
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

