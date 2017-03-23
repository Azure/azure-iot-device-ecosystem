---
platform: windows server 2008 r2
device: hitachi hf-w7500 model 40
language: c
---

Run a simple C sample on Hitachi HF-W7500 Model 40 device running Windows Server 2008 R2
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)


<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect HF-W7500 device running  Windows Server 2008 R2 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites
You should have the following items ready before beginning the process:
-   [Prepare your development environment][setup-devbox-windows]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   HF-W7500 device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Install KB2999226 on your device
-   Install Visual C++ Redistributable for Visual Studio 2015 on your device

<a name="Build"></a>
# Step 3: Build SDK and Run the sample

-   Start a new instance of Visual Studio 2015. Open the **azure_iot_sdks.sln** solution in the **cmake** folder in your home directory.

-   In Visual Studio, in **Solution Explorer**, navigate to **simplesample_http** project, open the **simplesample_http.c** file.

-   Locate the following code in the file:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   In **Solution Explorer**, right-click the **simplesample_amqp** project, click **Debug**, and then click **Start new instance** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
