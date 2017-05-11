---
platform: windows embedded standard 7
device: hf-w100e
language: c
---

Run a simple C sample on HF-W100E device running Windows Embedded Standard 7
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

This document describes how to connect HF-W100E device running  Windows Embedded Standard 7 with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-windows]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   HF-W100E device.


<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Install KB2999226 on your device.
-   Install Visual C++ Redistributable for Visual Studio 2015 on your device.

<a name="Build"></a>
# Step 3: Build and Run the sample

# 3.1 Build and Sample

-   Download the Azure IoT SDK and the sample programs and save them to your local repository.

-   Start a new instance of Visual Studio 2015.

-   Open the azure_iot_sdks.sln solution.

-   In Visual Studio, from **Solution Explorer**, navigate to the **samples** folder.

-   In the **simplesample\_http** project, open the **simplesample_http.c** file.

-   Locate the following code in the file:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Build the **simplesample_http** project and run the Device Client Http Sample application on your device

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
