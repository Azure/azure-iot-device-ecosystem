---
platform: no os
device: iot keystone innovator board
language: c
---

Configure the IoT.Keystone Innovator Board - _Certified for Azure IoT variant_ - to deliver sensor data securely to your Azure IoT Hub.
===
---
[Buy IoT.Keystone Innovator Board - Certified for Azure IoT variant](https://thisisiot.io/products/iot-keystone-innovator-board)

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Provision the Device](#Provision)
-   [Step 4: Send Measurements to your Hub](#Measurements)
-   [Tips](#tips)
-   [Next Steps](#NextSteps)


<a name="Introduction"></a>
# Introduction

IoT.Keystone is the easiest way to get sensor data to your Azure IoT Hub instance.

Unlike most other development boards, your IoT.Keystone Certified for Azure IoT variant of the IoT.Keystone Innovator Board does not require any coding, building or loading of firmware! No downloading of toolchains and no running of build scripts.  It has never been easier to securely move sensor data to the cloud. 

All you'll need to do is provision the device over a serial console with elements of the connection string.   Unbox, plug the board into a free USB port on your PC, connect your terminal emulator and input your connection string elements using the board's built-in console configuration system, as described below.

**About this document**

This document describes how to connect the IoT.Keystone Innovator Board Certified for Azure IoT device running the pre-loaded firmware. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Provisioning your IoT device's connection string 

In a nutshell, you need to have an Azure IoT Hub instance up, and an IoT device provisioned in the Hub.  Then grab the device's connection string and set it in the IoT.Keystone board.

<a name="Prerequisites"></a>
## Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Terminal emulator such as PuTTy or TeraTerm.  Ensure your terminal is configured to send Line Feed (LF), or CR+LF at the end of each command you type in.
-   [Setup your IoT hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md)

<a name="PrepareDevice"></a>
## Step 2: Access the serial shell and get the device ID.

You can use any device ID when you create a new device in Azure IoT Hub, but we require using the device's unique **DevEUI** string, consisting of 16 hex-chars representing an 8 byte unique ID, when connecting an IoT.Keystone Innovator Board.

On the console, type **identify** to get the Device ID (DevEUI) string:

    #0012.4b00.18a7.f322> identify
    DevEUI: 00124B0018A7F322
    #0012.4b00.18a7.f322>


-   [Create your device and get its connection string](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md).  

In this example, you should use **00124B0018A7F322** as the device ID! 

The connection string will look like (for example):

    HostName={your hub ID}.azure-devices.net;DeviceId={Your Device ID};SharedAccessKey={Your Shared Access Key}

Isolate these 2 important elements of the connection string as these will be used to provision your IoT.Keystone board for your Azure IoT Hub:

1.  {your hub ID}.  Example: `contoso`.
2.  {your shared access key}.  Example: `ABCaoxABCDK85/peABC1105yjxFt6hABCDEFGHIJZZQ=`

<a name="Provision"></a>    
## Step 3: Provision the 2 connection string elements


    #0012.4b00.18a7.f322> azure set hub contoso
    OK
    #0012.4b00.18a7.f322> azure set key ABCaoxABCDK85/peABC1105yjxFt6hABCDEFGHIJZZQ=
    OK
    #0012.4b00.18a7.f322> azure set
    Azure IoT Connection Settings:
    Hub: contoso.azure-devices.net
    Port: 8883
    SAS Token: SharedAccessSignature sr=contoso.azure-devices.net%2Fdevices%00124B0018A7F322&sig=ABCDEFGABCDEFGPxZqZafbg%2FwX6l8z%2Bo%2F7hU%3D&se=1581986104

The IoT.Keystone provisioning process accepts the (secret) shared access key.  This key is never transmitted to the IoT Hub or reported to the console after it is entered.  Rather, a **SAS token** is generated and this is what is sent in the MQTT connection request, and printed to the console when requested.  The SAS token is bound to your hub and device ID, and is valid for 365 days from when the key was entered via the command `azure set key`.  See above for an example of what the SAS token looks like.  You don't need to record it or know what it is, it is just a unique identity that your device uses to identify itself to the Azure IoT Hub without actually sending a secret key over the link (even though the link is TLS encrypted Microsoft uses this extra layer). 

<a name="Measurements"></a>
## Step 4: Send Sensor Measurements to your Hub

In this section you will use the IoT.Keystone Innovator Board Azure IoTto send sensor data to your IoT Hub instance.

You can also send arbitrary **command** strings to the device, and it will print them to the console.

By default every 10 seconds the IoT.Keystone board will send temperature, pressure, humidity, light and orientation measurements from the board's BME-280, OPT3001 and ICM-20948 sensors to the Azure IoT Hub.  This rate can be configured via the board's console. How often your board sends measurements factors greatly into battery life.  This is because measurements need to activate the processor and sensors, which consumes energy, plus also needs to activate the radio transmitter for additional energy consumption.

You can also setup measurement triggers based on the ICM-20948's built-in motion event and gesture processing engine.  For example, setup a trigger on pick up, dropping, or vibration activities.  When these triggers are setup, the ICM-20948 is turned on in a lower power state to monitor for these events, but this state-of-art motion processor sips current and you can still get months or years out of a suitable battery.

<a name="Tips"></a>
# Tips

Before you will see any data from your IoT.Keystone Innovator Board on your Azure IoT Hub instance, your IoT.Keystone board must be within range of a gateway and associated with it.  This process is automatic and a successful association is indicated by regular brief pulses of the green LED.  The process could take up to a minute to complete, and while it is doing so the green LED will toggle at 1 Hz.  

The gateway can be another IoT.Keystone Innovator Board configured in SLIP mode connected to a Linux host such as a Raspberry Pi offering the appropriate internet backhaul.  This gateway system requires running the `tunslip6` utility on the Pi and plugging an IoT.Keystone Innovator Board in SLIP mode into a USB port.  


We also offer an easy-to-use ethernet-backhaul gateway that requires no configuration and will automatically create and manage the wireless network for all IoT.Keystone boards within range. 

<a name="NextSteps"></a>
# Next Steps

You have now learned how to configure the IoT.Keystone Innovator Board - Certified for Azure IoT variant - to connect and report measurements to your IoT hub. To explore how to store, analyze and visualize the data from the IoT.Keystone Innovator Board in Azure using a variety of different services, please click on the following lessons:

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