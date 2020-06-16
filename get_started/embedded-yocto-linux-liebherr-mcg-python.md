---
platform: embedded linux
device: liebherr mcg
language: python
---

Run a simple PYTHON sample on Liebherr MCG device running Embedded Linux to connect to Azure IoT
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

This document describes how to connect the Liebherr MCG device running Embedded Linux with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Run the example script

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-python]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Liebherr MCG device.
-   Tool to connect to MCG via SSH (e.g. command line client or Putty)

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-   There is no need to build and deploy Azure IoT SDK it's already installed on the device
-   For a startup guide and technical information to MCG and other Liebherr Telematic products check

      [www.liebherr.com/telematics-integration](https://www.liebherr.com/telematics-integration)

## 2.1 Connect to MCG
-   Ensure your host system is connected to the MCG via Ethernet 
-   Use ssh to connect to the MCG using the default IP address       

        ssh root@192.168.3.48

## 2.2 Prepare the Example Script
-   Navigate to the location of the example script
-   Edit the example script azuremonitor<nolink>.py with your favorite editor (e.g. vi, nano or joe) 

        cd /opt/leg/cloud
        nano azuremonitor.py

-   Search for the definition CONNECTION_STRING
-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

<a name="Build"></a>
# Step 3: Run the sample
<a name="Load"></a>
## 3.1 Start the Script

-   Navigate to the location of the example script and
-   Start the example script, it runs continuously until it's stopped with CTRL+C

        cd /opt/leg/cloud

        ./azuremonitor.py

## 3.2 Send Device Messages to IoT Hub:

-   As long as the example script is continuously running it sends the following messages the the IoT Hub
    -   Battery voltage
    -   Analog input voltage 
    -   Device orientation (x, y, z based on acceleration sensor )
    -   Temperature
-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   As long as the example script is continuously running it prints received messages from IoT Hub to standard out
-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

## 3.4 Receive device twin update from IoT Hub
-   As long as the example script runs continuously it prints received device twin updates from IoT Hub to standard out

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
[setup-devbox-python]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/get_started/python-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

