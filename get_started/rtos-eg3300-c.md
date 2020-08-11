---
platform: rtos
device: eg3300
language: c
---

Connect, Capture and Communicate wide range of sensor data to Azure IoT Hub from EG3300
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Pre-requisites](#Pre-requisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Update Config Files](#ConfigFile)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect Cyient EG3300 device running on RTOS with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Update config file to device to set Azure credentials

<a name="Pre-requisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Buy EG3300 from Cyient][Cyient-EG3300]
-   Update Credentials in EG3300  

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   The Modular IoT Gateway EG3300 is a ready-to-deploy, industrial-grade, gateway for advanced Internet of Things applications. Modular IoT Gateway EG3300 supports wireless applications including GPS, Wi-Fi, and 2G/3G/4G cellular and wired connectivity such as USB 2.0, Gigabit Ethernet, serial ports, CAN 2.0b, analog inputs, and isolated digital I/O.
-   Read the manual to power on device and make sure it is connected to internet via Wi-Fi or Ethernet.
-   Configure the EG3300 with information by accessing the Webserver portal (ex: like any typical wifi router configuration) that would be provided with purchase of EG3300
-   Gateway comes with pre installed software for Azure sdk that has inbuilt methods for communicating with Azure through D2C messaging and Direct method.

<a name="ConfigFile"></a>
# Step 3: Load the Azure IoT Connection string  on device

-   Gateway provides Web interface for updating Azure IoT Hub connection string
-   Read the manual provided with Gateway for opening Webserver
-   Create Input and output connectors and Azure IoT Hub connection string on the Webserver
-   Connect sensors needed for collecting the data and pushing to Azure IoT Hub
-   Input connectors are data pipe lines for reading sensor data
-   Output connectors are the pipelines for updating data to Azure cloud
-   Once changes are done, submit the form and device will restart and will start pushing data to Azure IoT Hub

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. 
To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

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
[Manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[Save IoT Hub messages to Azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[Use Power BI to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[Remote monitoring and notifications with Logic Apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[Cyient-EG3300]: https://cdn2.hubspot.net/hubfs/5724847/FY_19_Revamp_Assets_Website/Resource%20Center/Flyers/Communications/iot-edge-gateways-flyer-0619.pdf
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

