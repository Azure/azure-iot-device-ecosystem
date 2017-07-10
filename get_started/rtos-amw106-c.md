---
platform: rtos
device: amw106
language: c
---

Run a simple C sample on AMW106 device running RTOS
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

In this guide, we will learn how to use ZentriOS Application (ZAP) running on Zentri device to communicate with Microsoft Azure IoT Hub. The ZAP publishes sensor readings to Azure. At the same time, a third party application is used to control the device (in our tests, we used Device Explorer).

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Computer with Git client installed and access to the
    [azure-iot-sdk-csharp](https://github.com/Azure/azure-iot-sdk-csharp) GitHub public repository.
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Zentri AMW106 Moray evaluation board [more info here](https://docs.zentri.com/hardware/zentri/amw106/amw106-e03)
-   Zentri DMS account [you can request one here](https://dms.zentri.com/signup)
-   Basic knowledge of [Device Explorer tool](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md)
-   Basic knowledge of [IoT Hub Explorer command line tool](https://github.com/azure/iothub-explorer) 
-   Preferred but not necessary: basic knowledge of MQTT messaging protocol
-   Before you start, we recommend getting yourself familiar with Azure IoT hub and MQTT support [here](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-mqtt-support)

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

1.  Obtain your AMW106 device UUID via the serial command:

        get system.uuid

2.  Go to: <https://portal.azure.com>
3.  Obtain your account's *hub name*, e.g. MyHub 
4.  If not existing, add a new device with *Device ID* matching your device UUID obtained above
5.  Once created, obtain the device's *Primary Key*

<a name="Build"></a>
# Step 3: Build and Run the Sample

<a name="Step_3_1_Build"></a>
## 3.1 Device side ZentriOS application (ZAP)

It is the time now to get the device side application (ZAP) up and running. Will explain here how to get the application from Zentri DMS, then how to configure it.

### Getting the ZAP:

1.	Assuming you already created DMS account, plug your Moray AMW106 evaluation board to your computer and open a serial terminal
2.	Claim your device if not claimed yet

        dms claim <dms_username> <dms_password>

3.	Activate with this tutorial firmware

        dms activate ZENTRI-AZUREIOTHUB

4.	Update to latest firmware release

        ota -f

    [more info here](https://docs.zentri.com/wifi/cmd/latest/commands#dms)

5.	Once updated, you will get a message indicating that operation succeeded: *OTA completed successfully*

### Configure the device:

1.  Set your WLAN SSID

        set wlan.ssid <your_ssid>

2.  Set network password

        set wlan.passkey <password>

3.  Set your hub name ID

        set azure.hub_name <you_iot_hub_name>

4.  Set your device key created above

        set azure.device_key <device-primary-key>

5.  Save parameters

        save

6.  Reboot your device

        reboot


<a name="Step_3_2_Run"></a>
## 3.2 Running the application

### Device side:

Once device reboots, the application will automatically start, and will execute the following sequence:

1.  Bring up WLAN network connection
2.  Establish an MQTT connection with the IoT Hub broker (secure connection):

        <your_hub_name>.azure-devices.net:8883

3.  Once connection established, the device will subscribe to the command topic over the MQTT messaging protocol

      devices/<your_device_name>/messages/devicebound/#  

4.  At this stage, the device will wait for commands to be received on the topic.

5.  Periodically, the device will publish messages to Azure broker. Each message gives the device status: windSpeed, temperature, and humidity. 

    ***Note:*** *the data is just a set of random values for demonstration.*

6.  The console will give informative log as well with all parameters.

### Device Explorer side:

1.  Download and install DeviceExplorer.
2.  Add connection information under the Configuration tab and click the Update button.
3.  Click the Management tab. Your registered devices will be visible in the list. In case your device is not there in the list, click Refresh button. 
4.  To verify that your IoT hub receives data from device, navigate to Data tab. Select the device name you created from the drop-down list of device IDs and click Monitor button.
5.  DeviceExplorer is now monitoring data sent from the selected device to the IoT Hub.
6.  Status data sent by device will be displayed for you.
7.  For transmitting path, go to the Messages to Device tab.
8.  Select the device you created using Device ID drop down.
9.  In the Message field, you can send whatever message to your device.
10. Device will receive the message and start parsing displaying it on the console. The app can then parse these messages to react accordingly.

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

