---
platform: Ubuntu
device: ADLE3800PC
language: c
---

Run a simple C sample on ADLE3800PC device running Ubuntu
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)

# Instructions for using this template

-   Replace the text in {placeholders} with correct values.
-   Delete the lines {{enclosed}} after following the instructions enclosed between them.
-   It is advisable to use external links, wherever possible.
-   Remove this section from final document.

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect ADLE3800PC device running Ubuntu with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-   Install Ubuntu 14.04 on [ADLE3800PC](http://www.adl-usa.com/product/adle3800pc/)

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

-   Open a PuTTY session and connect to the device.

-   Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by issuing the following commands from the command line on your board:

        sudo apt-get update

        sudo apt-get install -y curl uuid-dev libcurl4-openssl-dev build-essential cmake git

    Note: This setup process requires cmake version 2.8.12 or higher.

    You can verify the current version installed in your environment using the following command:

        cmake --version

    This library also requires gcc version 4.9 or higher. You can verify the current version installed in your environment using    the following command:

        gcc --version 

    For information about how to upgrade your version of gcc on Ubuntu 14.04, see http://askubuntu.com/questions/466651/how-do-i-use-the-latest-gcc-4-9-on-ubuntu-14-04.

-   Download the Microsoft Azure IoT Device SDK for C to the board by issuing the following command on the board::

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Edit the following file using any text editor of your choice:
   
    For AMQP protocol:

        azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp.c

    For HTTPS protocol:

        azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http.c

    For MQTT protocol:

        azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Build the SDK using following command.

        sudo ./azure-iot-sdk-c/build_all/linux/build.sh

## 3.2 Send Device Events to IoT Hub:

-   Run the sample by issuing following command:

    If using AMQP protocol:

        ~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp

    If using HTTP protocol:

        ~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http

    If using MQTT protocol:

        ~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.


[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../../setup_iothub.md
[lnk-manage-iot-hub]: ../../manage_iot_hub.md
