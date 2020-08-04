---
platform: ubuntu core 18
device: tallypoint rf-1
language: python
---

Run a simple RFID test on TallyPoint RF-1 device running Ubuntu Core 18
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Execute Manual Tests](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect the TallyPoint RF-1 device running Ubuntu Core 18  with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Testing your device with Azure IoT

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-python]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   [TallyPoint RF-1 device][lnk-tallypoint-rf1]

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Follow the instructions to [set up your TallyPoint reader][lnk-tallypoint-setup]. Plug in Ethernet POE+, or Ethernet and DC power supply.
-   Wait for the LED to change to solid green.
-   Open the TallyPoint web user interface using a web browser. You can find the IP address of the reader using your network control panel (e.g. UniFi), or use the TallyPoint multicast-DNS capability where the name on the network is tallypoint-0a0b0c or tallypoint-0a0b0c.local, where 0a0b0c is the last 3 hexadecimal digits of the MAC address printed on your reader's label.
-   Login to the reader using the default username of 'admin' and password of 'tallypoint'.
-   Change the password from the Reader tab in the interface.

<a name="Build"></a>
# Step 3: Execute Manual Tests

<a name="Load"></a>
## 3.1 Configure Azure device connection string

-   Select the Cloud tab in the web user interface.
-   Copy the Azure connection string created in Step 1 to the text box in the web interface.
-   Click the Azure Enable button in the web interface.
-   Click the Save button, then the Restart button.

## 3.2 Send Device Events to IoT Hub:

-   The TallyPoint RFID Edge Reader is now ready to send data to Azure IoT Hub. Place a supplied RAIN or UHF Gen2 RFID tag in front of the reader. The tag information is sent to your IoT Hub.
-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.
-   The TallyPoint reader currently supports the following cloud-to-device messages, all of these with no arguments:
    -   getReaderInfo
    -   getAntennaConfigs
    -   getSettings
    -   getVisibleTags

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
[lnk-tallypoint-rf1]: https://www.sdgsystems.com/tallypoint-rf1
[lnk-tallypoint-setup]: https://docs.google.com/document/d/1qqJqnwIUTlozCAL8Coeb_yh-a9YzvTrVRkT8Qdik60A/edit#heading=h.hlo66ji377nv
