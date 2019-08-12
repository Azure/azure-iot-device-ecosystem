---
platform: ti rtos or free rtos
device: msp432e4 launch pad
language: c
---

Run a simple C sample on [MSP432E4 Launchpad](http://www.ti.com/tool/MSP-EXP432E401Y) device.
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

This document describes how to connect [MSP432E4 Launchpad](http://www.ti.com/tool/MSP-EXP432E401Y) device running TI-RTOS or FreeRTOS to Microsoft Azure IoT Hub using the Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

 These instructions apply specifically to a user who is using TI’s SimpleLink MSP432E4 SDK and SimpleLink MSP432e4 SDK Plugin for Azure IoT software found at [dev.ti.com](http://dev.ti.com/tirex/#/). The Quick Start Guide that is included with your Plugin installation (you should be able to access this via the "docs" folder of the Azure SDK Plugin installation) provides more accurate instructions that apply specifically to the Plugin installation. 

 Alternatively, there are instructions in the [README.md](https://github.com/TexasInstruments/azure-iot-pal-simplelink/blob/master/README.md) for a user who has directly cloned or downloaded the [“azure-iot-pal-simplelink”](https://github.com/TexasInstruments/azure-iot-pal-simplelink) PAL repository. Please note that it is highly recommended to follow these TI SDK Plugin for Azure IoT instructions, versus cloning or downloading “azure-it-pal-simplelink” PAL repository directly, if you are just getting started.  The PAL repository provides a basic example running on TI-RTOS with makefiles for the TI compiler.  Full support for TI, GCC and IAR toolchains and FreeRTOS is provided via the Plugin installation.

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-windows]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   [MSP432E4 Launchpad](http://www.ti.com/tool/MSP-EXP432E401Y) device.
-   [TI SimpleLink MSP432E4 Software Development Kit](http://www.ti.com/tool/simplelink-msp432-sdk) v3.20 or compatible
-   [TI SimpleLink MSP432E4 SDK Plugin for Azure IoT](http://www.ti.com/tool/simplelink-sdk-plugin-for-azureiot)

Please ensure that your device has been updated with the latest firmware and or service pack (instructions for updating the firmware and/or service pack are included with the SimpleLink SDK installation).

If you are new to TI’s SimpleLink MCU platform and unfamiliar with the [MSP432E4 Launchpad](http://www.ti.com/tool/MSP-EXP432E401Y) it is recommended that you start with [TI’s SimpleLink Academy](http://dev.ti.com/tirex/#/?link=Software%2FSimpleLink%20MSP432E4%20SDK%2FSimpleLink%20Academy) Introduction.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Install [TI SimpleLink MSP432E4 Software Development Kit](http://www.ti.com/tool/simplelink-msp432-sdk) v3.20 or compatible

-   Install [TI SimpleLink MSP432E4 SDK Plugin for Azure IoT](http://www.ti.com/tool/simplelink-sdk-plugin-for-azureiot)

-   Follow the Quick Start Guide included with the TI SimpleLink MSP432E4 Plugin for Azure IoT

<a name="Build"></a>
# Step 3: Build SDK and Run the sample

-   [Building and running Examples in CCS](http://dev.ti.com/tirex/#/?link=Software%2FSimpleLink%20SDK%20Plugins%2FSimpleLink%20MSP432E4%20SDK%20Azure%20IoT%20Plugin%2FDocuments%2FQuick%20Start%20Guide)
-   [Building and running Examples in IAR](http://dev.ti.com/tirex/#/?link=Software%2FSimpleLink%20SDK%20Plugins%2FSimpleLink%20MSP432E4%20SDK%20Azure%20IoT%20Plugin%2FDocuments%2FQuick%20Start%20Guide)
-   [Building with makefiles](http://dev.ti.com/tirex/#/?link=Software%2FSimpleLink%20SDK%20Plugins%2FSimpleLink%20MSP432E4%20SDK%20Azure%20IoT%20Plugin%2FDocuments%2FQuick%20Start%20Guide)

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
[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
