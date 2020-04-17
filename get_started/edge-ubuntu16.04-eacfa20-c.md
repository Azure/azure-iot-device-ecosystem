---
platform: edge ubuntu16.04
device: eacfa20 
language: c
---

Run a simple C sample on EACFA20 device running Ubuntu16.04
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Manual Test for Azure IoT Edge on device](#Manual)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect EACFA20 device running C with Azure IoT Edge Runtime pre-installed and Device Management. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and Deploy client component to test device management capability 

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Sign up to IOT Hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Add the Edge Device](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux)
-   [Add the Edge Modules](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux#deploy-a-module)
-   EACFA20 device.
-   Hdmi panel and hdmi cable

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   EACFA20 device.

<a name="Manual"></a>
# Step 3: Manual Test for Azure IoT Edge on device

This section walks you through the test to be performed on the Edge devices running the Linux operating system such that it can qualify for Azure IoT Edge certification.

<a name="Step-3-1-IoTEdgeRunTime"></a>
## 3.1 Edge RuntimeEnabled (Mandatory)

**Details of the requirement:**

The following components come pre-installed or at the point of distribution on the device to customer(s):

-   Azure IoT Edge Security Daemon
-   Daemon configuration file
-   Moby container management system
-   A version of `hsmlib` 

*Edge Runtime Enabled:*

**Check the iotedge daemon command:** 

Open the command prompt on your IoT Edge device , confirm that the Azure IoT edge Daemon is under running state

    systemctl status iotedge

Open the command prompt on your IoT Edge device, confirm that the module deployed from the cloud is running on your IoT Edge device

    sudo iotedge list

On the device details page of the Azure, you should see the runtime modules - edgeAgent, edgeHub and tempSensor modueles are under running status


<a name="Step-3-2-DeviceManagement"></a>
## 3.2 Build and Run the sample (Optional)

**Pre-requisites:** Device Connectivity.

**Description:** The Microsoft Azure IoT device libraries for C contain code that facilitates building devices and applications that connect to and are managed by Azure IoT Hub services.

-   This repository contains various C sample applications that illustrate how to use the Azure IoT device SDK for C:

   -  [Simple samples for the device SDK][simple_samples]

   -  [Samples using the serializer library][serializer_samples]
-	Download the [Azure IoT SDK](https://github.com/Azure/azure-iot-sdk-c) and the sample programs and save them to your local repository.
-	Prepare your development environment use the [Microsoft Azure IoT device SDK for C](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux).
-	To run the sample applications, open a shell or command prompt window and navigate to the **azure-iot-sdk-c/iothub_client/samples/iothub\_ll\_telemetry\_sample** folder in the C project you downloaded. Then Edit the sample file iothub\_ll\_telemetry\_sample.c entering the proper connection string from the Azure Portal:

	static const char* connectionString = "[device connection string]";

-   Navigate to the directory of the sample that was edited, build the sample with the new changes and execute the sample:

    cd ./azure-iot-sdk-c/cmake/iothub_client/samples/iothub_ll_telemetry_sample
    make
    ./iothub_ll_telemetry_sample
  
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[simple_samples]:https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples
[serializer_samples]:https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples