---
platform: windows-8.1-embedded-industry-enterprisedevice: el20
device: el20
language: csharp
---

Run a simple sample on EL20 device running a Windows Operating System
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Step-1:-Prerequisites)
-   [Step 2: Prepare the Device](#Step-2:-PrepareDevice)
-   [Step 3: Build and Run the Sample](#Step-3:-Build)

<a name="Introduction"></a>
# Introduction

**About this document**

This document provides step-by-step guidance on how to connect an EL20 device running **a HPE Azure Certified Windows Operating
System** with Azure IoT SDK. A HPE Azure Certified Windows operating system is one in which a number of tests were run with the
EL20 device and the test results were submitted to Microsoft for certification. These include Windows 8.1 Embedded Industry Enterprise,
Windows 10, Windows Server 2012R2, and Windows Server 2016. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering the IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Step-1:-Prerequisites"></a>
# Step 1: Prerequisites

-   An EL20 device,
-   An empty USB drive,
-   A Computer with:
    -    GitHub installed
    -    Access to the Azure IoT SDK GitHub private repository located at: [azure-iot-sdks](https://github.com/Azure/azure-iot-sdks)
    -    Access to the Azure IoT SDK CSharp GitHub private repository located at: [azure-iot-sdk-csharp](https://github.com/Azure/azure-iot-sdk-csharp)
    -   [Visual Studio 2015](https://visualstudio.com/downloads/download-visual-studio-vs.aspx) installed
    -   [Microsft Azure SDK](https://microsoft.com/download/details.aspx?48178) installed,
-   [Setup the IoT hub][lnk-setup-iot-hub],
-   [Provision the device and get its credentials][lnk-manage-iot-hub]

<a name="Step-2:-PrepareDevice"></a>
# Step 2: Prepare the Device
##  Install Windows Operating System on EL20
-   Obtain an iso for the specific operating system and copy it to the USB drive,
-   Make the USB drive bootable. Follow this guide on how to create a bootable drive (<https://www.microsoft.com/en-us/download/windows-usb-dvd-download-tool>),
-   Insert the bootable USB Drive from the previous step into the EL20,
-   Turn on the EL20 device and press the **Delete** key,
-   Change the BIOS Boot option filter to **UEFI and Legacy**,
-   Change the **Boot Option Priorities** to boot from the USB Drive,
-   Save changes and restart the EL20,
-   Follow the on screen instructions to install Windows Operating System on the EL20

<a name="Step-3:-Build"></a>
# Step 3: Build and Run the sample

-   Clone the Azure IoT SDK,
-   Clone the Azure Iot SDK CSharp,
-   Start a new instance of Visual Studio 2015,
-   Open the **iothub_csharp_deviceclient.sln** solution in the `device` folder in the local copy of the repository,
-   In Visual Studio, from Solution Explorer, navigate to the **samples** folder,
-   In the **DeviceClientAmqpSample** project, open the ***Program.cs*** file,
-   Locate the following code in the file:
        private const string DeviceConnectionString = "<replace>";        
-   Replace `<replace>` with the connection string for the device,
-   In visual Studio, under Solution Explorer:
    -    right-click the **DeviceClientAmqpSample** project,
    -    click ***Debug &minus;&gt; Start new instance*** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub,
-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application and how to send cloud-to-device messages to the application.

[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
