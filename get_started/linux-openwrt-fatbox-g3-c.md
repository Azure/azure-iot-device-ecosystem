---
platform: linux 3.0.35 openwrt
device: fatbox g3
language: c
---

Configure FATBOX G3 as a LTE device gateway for Azure IoT
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Test the IoT client](#Test)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to configure FATBOX G3 as device gateway for Azure IoT with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Test your device

<a href="http://www.amplified.com.au/#!4gltegateway/c192n">FATBOX G3</a>

An industrial LTE gateway device to allow secure remote monitoring and management of industrial equipment with Modbus RTU, Modbus TCP, RS-485 or CAN bus using the Azure IoT platform.

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   FATBOX G3 device
-   Save your device connection string into a text file named connstr.txt. It will be used to setup and configure your FATBOX G3 as a secure device to connect to your IoT Hub.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Login to your FATBOX G3 using the LAN port.

    We suggest 2 ways to upload your unique Azure IoT Hub device connection string to your FATBOX.

    1)  Using a USB drive (with micro USB connector)
        -   Copy the connstr.txt file into your USB drive /user folder.
	-   Insert USB drive into FATBOX G3's USB port in the rear.
        -   From 'Management', click on 'Download to FATBOX' and check that 'OK' led on FATBOX blinks once.
	
	    ![](media/download_to_fatbox.png)
	    
    2)  Using SCP or WINSCP over Ethernet connection
        -   Enable SSH on the FATBOX (reboot after 'Update').
	-   Use SCP or WINSCP to transfer the connstr.txt from your computer to the FATBOX /user folder. 
-   In the FATBOX G3 'Azure IoT' menu, enable the Azure IoT device client option.

            ![](media/fatbox_enable_device_client.png)
	    

<a name="Test"></a>
# Step 3: Test the IoT client

## 3.1 Send Device Events to IoT Hub:

-   SSH to the FATBOX (using root and password as setup in your LOGIN).
-   Create or copy a few lines of 'test data' into the /tmp/dataq.txt file.
-   Run the /etc/M2MBOT in the FATBOX.
-	'Test data' will be sent line by line by the IoT client to your Azure IoT Hub  
-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.2 Receive messages from IoT Hub

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

