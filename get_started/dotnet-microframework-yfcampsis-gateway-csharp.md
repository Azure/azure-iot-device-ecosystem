---
platform: dotnet microframework
device: iot.net field gateway
language: csharp
---

Run a simple Csharp sample on YFSoft Campsis Gateway device running .Net Microframework
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

This document describes how to connect YFSoft Campsis Gateway device running .Net Microframework with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Computer with Git client installed and access to the
    [azure-iot-sdks](https://github.com/Azure/azure-iot-sdks) GitHub public repository.
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Innovactive YFSoft Campsis Gateway device.

#### Install Visual Studio 2015 and Tools

-   To create Windows .NET Micro Framework solutions, you will need to install [Visual Studio 2015](https://www.visualstudio.com/en-us/products/vs-2015-product-editions.aspx). You can install any edition of Visual Studio, including the free Community edition.

    After installing Visual Studio, goto Tools option in the menu bar and click on *Extensions and Updates*. Search for 'netmf' and install .NET Micro Framework SDK.

-   Depending on specific hardware revision of Innovactive YFSoft Campsis Gateway, YFSoft SDK could be needed in order to build and run provided sample. You can find YFSoft SDK <a href="https://pan.baidu.com/s/1i5uoKKH">here</a>:

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Provide a power supply for device, either through USB device socket or using provided external terminal connector (9~24V DC, 0.5A).

<a name="Build"></a>
# Step 3: Build and Run the Sample

<a name="Step_3_1_Connect"></a>
## 3.1 Connect the Device

-   Connect the board to your network using an Ethernet cable. This step is required, as the sample depends on internet access.

-   Plug the device into your computer using a micro-USB cable.

<a name="Step_3_2_Build"></a>
## 3.2  Build the Samples

-   Start a new instance of Visual Studio 2015. Open the **iothub_csharp_netmf_client.sln** solution (/azure-iot-sdks/csharp) from your local copy of the repository.

-   Download pre-configured project <a href="http://www.yfiot.com/Azure/YFSoft_Azure_HTTPs.rar">here</a>

-   In Visual Studio, from **Solution Explorer**, navigate to the **NetMFDeviceClientHttpSample_42 version>** project.

-   Locate the following code in the **Program.cs** file:

        public const string DeviceConnectionString = "<replace>";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   To build and deploy the binaries on your device, right-click on the project in the **Solution Explorer**, select **Properties** and navigate to the **.NET Micro Framework** tab.

    Select the **Transport** and **Device** which you have connected.

-   Build the solution.

<a name="Step_3_3_Run"></a>
## 3.3 Run and Validate the Samples

### 3.3.1 Send Device Events to IoT Hub

-   In Visual Studio, from **Solution Explorer**, right-click the sample project, click **Debug &minus;&gt; Start new instance** to build and run the sample.

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

### 3.3.2 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
