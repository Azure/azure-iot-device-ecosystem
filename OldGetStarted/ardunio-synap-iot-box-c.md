---
platform: ardunio
device: synap iot in a box
language: c
---

Get started with the Synap IoT in a Box sensor
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Setup your Device](#Setup)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Instructions

**About this document**

This document describes how to connect a Synap IoT Sensor to Microsoft Azure IoT Hub

-   Configuring Microsoft Azure IoT Hub
-   Install your Synap IoT Sensor
-   Registering your Synap IoT Sensor Device in Microsoft Azure IoT Hub
-   Start collecting Sensor Data

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Synap IoT in a Box sensor device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Unbox the sensor and connect the power cord
-   Use a Smart Phone to scan WiFi networks and connect to the Synap IoT Sensor. The WiFi name is Synap-<serial number>. The Serial number is located on the Sensor and on the packaging.
-   Connect to Synap IoT Sensor to the WiFi you want to Sensor to use.
-   Obtain the Sensors IP address by searching for the Mac address of the sensor. You can look at your DHCP Server or use arp -A and fin the Mac address and the corresponding IP address. The Sensors MAC address is located on the Sensor and on the packaging.


<a name="Setup"></a>
# Step 3: Setup the Sensor

-   Open a Browser and enter the IP address of the sensor.
-   In your browser, click on the Sensor Settings tab.
-   Enter a Friendly Name for the sensor.
-   Enter the interval in Seconds that the sensor will use between each Data transmission to Azure IoT Hub.
-   Enter in which timezone the Sensor is located. Enter the number of hours from GMT. 

    Example: New York, USA = -5 and Bangkok, Thailand = 7.

-   Enter Sensor Type. The Sensor Type is shown in the Device Twin in the IoT Hub, so it could be for example: Thermometer, Flow Sensor, Cryogen Freezer etc.
-   Enter Sensor Location. This will also be shown in the Device Twin. Example: Building 2, Office 6.
-   Enter Sensor Telemetry Data. This is shown in the Device Twin, and is a meant as a what measuring unit the data is in. It could be: Meter, Liter/per, Degree Celcius, Kg, Distance in cm etc.
-   Click on Save.

-   Now click on the Microsoft Azure IoT Hub Settings tab
-   Copy your Primary and Secondary Connection String from the Device in your Azure IoT Hub and paste them here.
-   Click on Save and then Restart


<a name="NextSteps"></a>
# Next Steps

The Sensor is now sending data to your Azure IoT Hub. Below you can find much more information about how to use the data in Microsoft Azure.
Please click on the following lessons:

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
