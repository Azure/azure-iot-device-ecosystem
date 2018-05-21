---
platform: windows 10 ltsb
device: ams300
language: csharp
---

Run a simple Csharp sample on AMS300 device running Windows 10 LTSB
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

This document describes how to connect AMS300 device running Windows 10 LTSB with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-windows]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   AMS300 device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Create a bootable USB Drive. Please follow this guide on how to create a bootable drive. ([https://www.microsoft.com/download/windows-usb-dvd-download-tool](https://www.microsoft.com/download/windows-usb-dvd-download-tool)).
-   Insert the bootable USB Drive from the previous step into your AMS300. Turn on your AMS300 device and press the **Delete** key.
-   Change the BIOS Boot option filter to “**Boot**”.
-   Change the “**Boot Option Priorities**” to boot from your USB Drive.
-   Save changes and restart your AMS300. Follow on screen instructions to install Windows Operating System on your AMS300.
-   In AMS300, install the latest .NET Core from [https://dot.net](https://dot.net)
-   In AMS300, install .NET Framework 4.7 Developer Pack: [https://support.microsoft.com/en-us/help/3186612/the-net-framework-4-7-developer-pack-and-language-packs](https://support.microsoft.com/en-us/help/3186612/the-net-framework-4-7-developer-pack-and-language-packs)
-   In AMS300, install .NET Framework 4.5.1 Developer Pack: [https://www.microsoft.com/en-us/download/details.aspx?id=40772](https://www.microsoft.com/en-us/download/details.aspx?id=40772)
-   In AMS300, as admin (one-time setup): Enable Powershell script execution on your system. See [http://go.microsoft.com/fwlink/?LinkID=135170](http://go.microsoft.com/fwlink/?LinkID=135170) for more information. 

    `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

<a name="Build"></a>
# Step 3: Build and Run the sample

-   Download the [Azure IoT SDK](https://github.com/Azure/azure-iot-sdk-csharp) and the sample programs and save them to your local repository.

-   Open a device console (command prompt or a powershell window) and change to your local SDK **azure-iot-sdk-csharp** directory.

-   Add the Iot Hub device connection string on your device as an environment variable:

	    setx IOTHUB_DEVICE_CONN_STRING <yourDeviceConnectionString>

-   Run the following command to build the SDK:

	    build.cmd -config Release

-   If build completed but no files is created under iothub\device\samples\DeviceClientHttpSample\ , try to modify build.ps1 to fix this issue. (add **BuildProject iothub\device\samples "IoT Hub DeviceClient Samples"** to building rules)
        
-   From the device console, run the sample using following command:

	**If HTTP protocol:**

		cd iothub\device\samples\DeviceClientHttpSample\bin\Debug\netcoreapp2.0
		dotnet DeviceClientHttpSample.dll

	**If MQTT protocol:**

		cd iothub\device\samples\DeviceClientMqttSample\bin\Debug\netcoreapp2.0
		dotnet DeviceClientMqttSample.dll
		
	**If AMQP protocol:**

		cd iothub\device\samples\DeviceClientAmqpSample\bin\Debug\netcoreapp2.0
		dotnet DeviceClientAmqpSample.dll

-   Use the **DeviceExplorer** utility to observe the messages IoT Hub receives from the **Device Client Sample** application.
-   Refer "Monitor device-to-cloud events" in [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to see the data your device is sending.
-   Refer "Send cloud-to-device messages" in [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) for instructions on sending messages to device.

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
[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
