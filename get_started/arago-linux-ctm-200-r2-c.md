---
platform: arago linux
device: ctm-200 r2
language: c
---

Run a simple C sample on CTM-200 R2 device running Arago Linux
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

This document describes how to connect CTM-200 R2 device running Arago Linux with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   CTM-200 R2 device.
-   CTM-200 R2 with firmware 2.6.0.5476 or later installed.
-   Valid Wireless connection via Cellular or Wi-Fi.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   The Azure libraries have been compiled and added to the CTM-200 as part of the IOT platform support on the CTM-200.
-   Use the unique IMEI of the device for registering device on Azure. To get the IMEI, run following command:

        cmd esn

-   On the CTM-200, enter following command:

        cmd iot azure connection_string "<connection string>"

-   The CTM-200 has a robust reporting engine capable of providing a wide range of connected sensor data in formatted reports via wireless. The sensor data consists of on-board sensors such as GNSS (GPS), Accelerometer and network parameters to external sensors such as RFID, GPIO, CAN Bus and auxiliary industrial controls. More information on the reporting and configuration of the CTM-200 can be found at: <http://www.cypress.bc.ca/resources>.

    ### Configure the report type

-   Report type "13" will configure the CTM-200 to send data to the Azure IOT cloud.  

    **Configure the CTM-200 to use GPS report #2 and reporting type 13 (Azure)**

        cmd gpsrep 2 0 13  

    Up to 8 default GPS condition and 999 general reports can be configured.  The report type defines the method of payload delivery, UDP, TCP, serial,cloud, etc.

    ### Configure the report condition

    **Sets the condition of GPS report 2 to send data every 60 seconds**

        cmd gpscond 2 1 60

    The CTM-200 reporting engine is highly configurable based on a variety of triggering event logic built into the CTM-200, this example uses a simplified “once every 60 second report”.  Condition or Trigger logic can be based on a variety of sensor input thresholds or time, speed, distance combinations.

    ### Configure the payload or message

    **add message 1000 ($PCYP) to GPS report 2**

        cmd gpsaddmes 2 1000

    The CTM-200 supports a wide range of standard and proprietary message strings.  For this example the $PCYP message is used.  $PCYP is a highly configurable “key”/”value” pair message that can be tailored to send specific data payloads depending on the type of sensors being monitored.  For this example, we will send the CTM-200 system firmware version (SYS\_FW\_LABEL) and the monitored on board system temperature (SYS\_TEMPERATURE).

-   Save and reboot the CTM-200 and gateway will now be reporting data to azure.

        cmd save
        cmd pwr mode 2


<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

-   The Azure libraries have been compiled and added to the CTM-200 as part of the IOT platform support on the CTM-200.

## 3.2 Send Device Events to IoT Hub:

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.


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
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

