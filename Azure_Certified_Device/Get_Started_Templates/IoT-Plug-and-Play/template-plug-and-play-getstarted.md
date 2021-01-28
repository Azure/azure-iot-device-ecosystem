---
platform: {enter the OS name running on device}
device: {enter your device name here}
language: {}
---

Connect {enter your device name here} device to your Azure IoT services
===

---
# Table of Contents

-   [Introduction](#Introduction)
-   [Prerequisites](#Prerequisites)
-   [Prepare the Device](#preparethedevice)
-   [Connect to Azure IoT Central](#ConnecttoCentral)
-   [Integration with Azure IoT Explorer](#IntegrationwithAzureIoTExplorer)
-   [Additional Links](#AdditionalLinks)

# Changelist from last update - Nov2020 (remove this section)
-   IoT Central section is a **mandatory** section that provides the value of IoT Plug and Play experience. The goal is to streamline the out of box experience
-   Using Azure IoT Explorer is **optional** 

# Instructions for using this template

-   Replace the text in {placeholders} with correct values.
-   Delete the lines {{enclosed}} after following the instructions enclosed between them.
-   It is advisable to use external links, wherever possible.
-   Remove this section from final document.

# Tips for authoring great getting started guide (remove this section)
Following below tips reduces operational overhead via email exchange and accelerate your overall certification process

- When there are multiple options to provision devices using DPS, try to define the golden path in the main flow. Put other paths in #Additional information section below
- Provide some paragraphs to the headers and avoid headers with just a link
- Device application must be either pre-installed on the device or download-able via various means (partner hosted website/GitHub etc). Be specific about the steps on deploying or flashing the device application
- Be specific about how to provision a device using DPS. DPS ID scope, registration ID and attestation methods (X.509, TPM or SAS key) configuration
- Provide estimated time to complete the end-to-end operation. For better experience, we recommend to put estimated time for each section in "Prepare your device", "Integration with Azure IoT Explorer" and "Integration with Azure IoT Central" respectively


<a name="Introduction"></a>

# Introduction 

**About this document**

This document describes how to connect {enter your device name here} to Azure IoT Hub using the Azure IoT Explorer with certified device application and device models.

IoT Plug and Play certified device simplifies the process of building devices without custom device code. Using Solution builders can integrated quickly using the certified IoT Plug and Play enabled device based on Azure IoT Central as well as third-party solutions.

This getting started guide provides step by step instruction on getting the device provisioned to Azure IoT Hub using Device Provisioning Service (DPS) and using Azure IoT Explorer to interact with device's capabilities.

{Please provide introduction and features of your device here}

<a name="Prerequisites"></a>
# Prerequisites

You should have the following items ready before beginning the process:

**For Azure IoT Central**
-   [Azure Account](https://portal.azure.com)
-   [Azure IoT Central application](https://apps.azureiotcentral.com/)


**For Azure IoT Hub**
-   [Azure IoT Hub Instance](https://docs.microsoft.com/en-us/azure/iot-hub/about-iot-hub)
-   [Azure IoT Hub Device Provisioning Service](https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision)
-   [Azure IoT Public Model Repository](https://docs.microsoft.com/en-us/azure/iot-pnp/concepts-model-repository)

<a name="preparethedevice"></a>
# Prepare the Device.

**Development Environmental setup**

IoT Plug and Play Certification is certifying specific device code implementation against specific device model. Device builders should either pre-install device code or make the binary download-able.{Please include the below pointers specific to device in this section and add screen shots where ever necessary}

1.	Describing the capabilities of the device 
2.	How to setup the device and connect power
3.	How to take the DPS configuration and program the device (Note : DPS ID scope should be configured w/o recompiling the embedded code)
4.	How to configure device over Wifi, cellular, screens, etc.
5.	Add the links of external software/tools as required 
6.	Add steps on how to run the device code/how and where to download binary and then run on device. If you have multiple options on how to deploy device code please mention only one option here and other options in Additional links section
7.	(Dev Kit requirement; Optional otherwise)
	Please include the following information to assist developers with your device:
	1)	Instructions for cloning the repo to download all sample device code, setup scripts and offline documentation
	2)	Instructions to install necessary tools and instructions to compile. Provide links to the tools to download (such as GCC, CMake etc.). 
	Provide version details if there are dependencies in compiling the code.
A well-written GSG can be [found here](https://github.com/azure-rtos/getting-started) for reference. If your device code uses a ported version of the Azure IoT C SDK, please identify which ported version is required.

8.  	How to configure a device to connect to Azure IoT Central (see detail below)

<a name="ConnecttoCentral"></a>
# Connect to Azure IoT Central (Simple)
This section is mandatory section. Describe how to connect to Azure IoT Central.
To configure a device to connect to Azure IoT Central you need the following
	
-    ID scope: In your IoT Central application, navigate to Administration > Device Connection. Make a note of the ID scope value.
-    Group primary key: In your IoT Central application, navigate to Administration > Device Connection > SAS-IoT-Devices. Make a note of the shared access signature Primary key value.

Use the Cloud Shell to generate a device specific key from the group SAS key you just retrieved using the Azure CLI

az extension add --name azure-iot
az iot central device compute-device-key  --device-id sample-device-01 --pk <the group SAS primary key value>

Make a note of the generated device key, and the ID scope for this application and flash it on the device

<a name="IntegrationwithAzureIoTExplorer"></a>
# Integration with Azure IoT Explorer (Advanced)
This section is optional for Advanced setup

-   Include the steps on how to connect the IoT Plug and Play Device to Azure IoT Explorer
-   Include screenshots and comments on how IoT Explorer shows/visualize telemetry , commands and properties coming from your IoT Plug and Play device.
-   Include the steps on how to interact with devices (telemetry, commands properties)
-   Ensure to attach the screenshot on consuming the device models available in public repository (not local folder) when using Azure IoT Explorer

# Additional information
Put any additional information here such as alternative paths to deploy device application etc.

<a name="AdditionalLinks"></a>
# Additional Links

Please refer to the below link for additional information for Plug and Play 

-   [Manage cloud device messaging with Azure-IoT-Explorer](https://github.com/Azure/azure-iot-explorer/releases)
-   [Import the Plug and Play model](https://docs.microsoft.com/en-us/azure/iot-pnp/concepts-model-repository)
-   [Configure to connect to IoT Hub](https://docs.microsoft.com/en-us/azure/iot-pnp/quickstart-connect-device-c)
-   [How to use IoT Explorer to interact with the device ](https://docs.microsoft.com/en-us/azure/iot-pnp/howto-use-iot-explorer#install-azure-iot-explorer)   
