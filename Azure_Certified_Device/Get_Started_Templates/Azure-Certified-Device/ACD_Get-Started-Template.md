---
platform: {enter the OS name running on device}
device: {enter your device name here}
language: {enter here}
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#Prepareyourdevice)
-   [Step 3: Build SDK and Run Samples](#Build)
-   [Step 4 : Integration with Azure IoT Explorer](#Explorer)
-   [Step 5: Connect to Azure IoT Central](#AzureIoTCentral)
-   [Step 6 : Additional Information](#AdditionalInformation)
-   [Step 7 : Additional Links](#AdditionalLinks)

# Instructions for using this template

-   Replace the text in {placeholders} with correct values.
-   Delete the lines {{enclosed}} after following the instructions enclosed between them.
-   It is advisable to use external links, wherever possible.
-   Remove this section from final document.

<a name="Introduction"></a>
# Introduction
**About this document**
This document describes how to connect {enter your device name here} device running {enter the OS name running on device} with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Provisioning your devices on Device Provisioning service 
-   Build and deploy Azure IoT SDK on device
-   Please provide introduction and features of your device here

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md)
-   [Setup your IoT hub](https://github.com/robertalorro/azure-iot-device-ecosystem/blob/master/setup_iothub.md)
-   [Provision your device over DPS](https://docs.microsoft.com/en-us/azure/iot-dps/about-iot-dps)

<a name="Prepareyourdevice"></a>
# Step 2: Prepare your Device
-    How to setup the device and connect power.
-    How to take the DPS configuration and program the device (Note: DPS ID scope should be configured w/o recompiling the embedded code) 
-    How to configure device over Wifi, cellular, screens, etc.
-    Add the links of external software/tools as required.

<a name="Build"></a>
# Step 3 : Build SDK and Run Samples

-    Add steps on how to run the device code, including how and where to download binaries. If you have multiple options on how to deploy device code, please mention only one option here and other options in Additional links section.
For Development Kits (optional otherwise), please include the following additional information to assist developers with your device:
-    Instructions for cloning the repo to download all sample device code, setup scripts and offline documentation.
-    Instructions to install necessary tools and instructions to compile. Provide links to the tools to download (such as GCC, CMake etc.). Provide version details if there are dependencies in compiling the code. A well-written GSG can be found [here](https://github.com/azure-rtos/getting-started) for reference. If your device code uses a ported version of the Azure IoT C SDK, please identify which ported version is required.

<a name="Explorer"></a>
# Step 4: Integration with Azure IoT Explorer

-   Include the steps on how to connect the IoT Device to Azure IoT Explorer
-   Include screenshots and comments on how IoT Explorer shows/visualize telemetry, commands and properties coming from your IoT Plug and Play device.
-   Include the steps on how to interact with devices (telemetry, Direct Methods and Cloud to Device Communication)

<a name="AzureIoTCentral"></a>
## Step 5: Connect to Azure IoT Central [“Required for Plug-and-play program; Optional otherwise”]

Describe how to connect to Azure IoT Central. To configure a device to connect to Azure IoT Central you need the following.

ID scope: In your IoT Central application, navigate to Administration > Device Connection. Make a note of the ID scope value.

Group primary key: In your IoT Central application, navigate to Administration > Device Connection > SAS-IoT-Devices. Make a note of the shared access signature Primary key value.

Use the Cloud Shell to generate a device specific key from the group SAS key you just retrieved using the Azure CLI

	az extension add --name azure-iot 
	az iot central device compute-device-key --device-id sample-device-01 --pk

Make a note of the generated device key and the ID scope. Then follow the instruction described in the ‘Prepare your device’ section about how to take the DPS configuration and program the device.

<a name="AdditionalInformation"></a>
# Step 6: Additional Information 
Put any additional information here such as alternative paths to deploy device application etc.

<a name="AdditionalLinks"></a>
# Step 7 : Additional Links
Please refer to the below link for additional information for Plug and Play

-   [Manage cloud device messaging with Azure-IoT-Explorer](https://github.com/Azure/azure-iot-explorer/releases)
-   [Azure SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/samples/prov_dev_client_sample/prov_dev_client_sample.c)
-   [Configure to connect to IoT Hub](https://docs.microsoft.com/en-us/azure/iot-pnp/quickstart-connect-device-c)
-   [How to use IoT Explorer to interact with the device](https://docs.microsoft.com/en-us/azure/iot-pnp/howto-use-iot-explorer#install-azure-iot-explorer)

