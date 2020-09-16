---
platform: {enter the OS name running on device}
device: {enter your device name here}
language: c
---

Connect {enter your device name here} device to your Azure IoT Central Application
===

---
# Table of Contents

-   [Introduction](#Introduction)
-   [Prerequisites](#Prerequisites)
-   [Create Azure IoT Central application](#Create_AICA)
-   [Device Connection Details](#DeviceConnectionDetails)
-   [Prepare the Device](#preparethedevice)
-   [Integration with IoT Central](#IntegrationwithIoTCentral)
-   [Additional Links](#AdditionalLinks)

# Instructions for using this template

-   Replace the text in {placeholders} with correct values.
-   Delete the lines {{enclosed}} after following the instructions enclosed between them.
-   It is advisable to use external links, wherever possible.
-   Remove this section from final document.

<a name="Introduction"></a>

# Introduction 

**About this document**

This document describes how to connect {enter your device name here} to Azure IoT Central application using the IoT plug and Play model. Plug and Play simplifies IoT by allowing solution developers to integrate devices without writing any device code. Using Plug and Play, device manufacturers will provide a model of their device to cloud developers to be integrated quickly into IoT Central or any solution built on the Azure IoT platform. IoT Plug and Play will be open to the community by way of a definition language and SDKs.

{Please provide introduction and features of your device here}

<a name="Prerequisites"></a>
# Prerequisites

You should have the following items ready before beginning the process: 

-   [Azure Account](https://portal.azure.com)
-   [Azure IoT Hub Instance](https://docs.microsoft.com/en-us/azure/iot-hub/about-iot-hub)
-   [Azure IoT Hub Device Provisioning Service](https://docs.microsoft.com/en-us/azure/iot-dps/about-iot-dps)
-   Provide Network connectivity (Wifi, LAN) supported by the device
-   Its mandatory that the device code/software image is preinstalled in device to enable Plug and Play
-   {Provide URL here to setup guide of development environment}

**Note:** If the device code is not preinstalled following are the [options](#preparethedevice) to choose to enable the plug and play device.

<a name="preparethedevice"></a>
# Prepare the Device.

**Hardware Environmental setup**

-   Please include how to setup and connect the device. Include external links for any software required for Hardware setup

**Software Environmental Setup**

-   Please include the prerequisite required to setup the device. Please use external links when possible pointing to your own page with device preparation steps.

### Option 1

-   If device code not pre-installed on your device, please provide us the URL of your repository.

-   Specify the steps on how to flash the image on device and provide required URL's to download the flash-able image and necessary tools. **Please add the screenshots where ever necessary.**

### Option 2

-   For the partners using the Microsoft PnP SDK samples

-   If recompilation of code is not required, please provide config file here, such that it can be copied in the C sdk environment to enable Plug and Play on device

-   Please include instruction on how to compile the code, tools and environment required to compile etc. **Please add the screenshots where ever necessary**

### Option 3

-   If recompilation is required, then please provide the link for GitHub repo for anyone to modify.

-   Please include instruction on how to compile the code , tools and environment required to compile etc. **Please add the screenshots where ever necessary**

<a name="IntegrationwithIoTCentral"></a>
# Integration with IoT Central

-   Include the steps on how to connect the Device to IoT Central
-   Include the steps by step process on how the devices use the DPS configuration (ID Scope, SAS Key, Device ID, Registration ID) to provision to IoT Central.
-   Include screenshots and comments on how IoT Central shows/visualize telemetry coming from your PnP device.
-   Use this [Get started]( https://aka.ms/AA66he8) doc as an example

<a name="AdditionalLinks"></a>
# Additional Links

Please refer to the below link for additional information for Plug and Play 

-    [Blog](https://azure.microsoft.com/en-us/blog/iot-plug-and-play-is-now-available-in-preview/)
-    [FAQ](TBD) 
-    [Plug and Play C SDK](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) 
-    [Plug and Play Node SDK](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview)
-    [Plug and Play Definitions](https://github.com/Azure/IoTPlugandPlay)

