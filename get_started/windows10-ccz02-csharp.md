---
platform: windows 10
device: CCZ02
language: csharp
---

Run a simple Csharp sample on CCZ02 device running Windows 10
===
---

# Table of Contents

-   [Introduction](#_Introduction)
-   [Step 1: Prerequisites](#Step_1:_Prerequisites)
-   [Step 2: Prepare your Device](#Step_２:_PrepareDevice)
-   [Step 3: Build and Run the Sample](#Step_3:_Build)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect CCZ02 device running Windows 10 with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment](https://catalog.azureiotsuite.com/docs?title=Azure/azure-iot-sdk-c/doc/devbox_setup)
-   [Setup your IoT hub](https://catalog.azureiotsuite.com/docs?title=Azure/azure-iot-device-ecosystem/setup_iothub)
-   [Provision your device and get its credentials](https://catalog.azureiotsuite.com/docs?title=Azure/azure-iot-device-ecosystem/manage_iot_hub)
-  CCZ02 device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Follow the instructions from [device website](http://www.weibu.com/)

<a name="Build"></a>
# Step 3: Build and Run the sample

-   Download the [Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) and the sample programs and save them to your local repository.
-   Start a new instance of Visual Studio 2015.
-   Open the **iothub_csharp_client.sln** solution in the `csharp\device` folder in your local copy of the repository.
-   In Visual Studio, from Solution Explorer, navigate to the **samples** folder.
-   In the **DeviceClientAmqpSample** project, open the ***Program.cs*** file.
-   Locate the following code in the file:

        private const string DeviceConnectionString = "<replace>";
        
-   Replace `<replace>` with the connection string for your device.
-   In **Solution Explorer**, right-click the **DeviceClientAmqpSample** project, click **Debug**, and then click **Start new instance** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.
-   Use the **DeviceExplorer** utility to observe the messages IoT Hub receives from the **Device Client AMQP Sample** application.


[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../../setup_iothub.md
[lnk-manage-iot-hub]: ../../manage_iot_hub.md
