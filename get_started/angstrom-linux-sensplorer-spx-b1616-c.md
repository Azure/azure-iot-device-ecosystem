---
platform: angstrom linux
device: sensplorer spx-b1616
language: c
---

Run a simple C sample on SENSPLORER SPx-B1616 device running customised LINUX Angstrom Distribution
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

This document describes how to connect SENSPLORER SPx-B1616 device running customised LINUX Angstrom Distribution with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   SENSPLORER SPx-B1616 device
-   Angstrom LINUX platform
-   Cross compiler settings:

        git clone https://github.com/sensplorer/spx_azure_iothub_c.git 

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Power up SENSPLORER SPx-B1616 Device and connect network cable to same network with your LINUX platform.
-   Connect SENSPLORER SPx-B1616 Device via ssh:

        ssh root@<IP address of SENSPLORER SPx-B1616>

    **Note:** Default IP address of SENSPLORER SPx-B1616 is 192.168.1.128.


<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

-   Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by issuing the following commands from the command line on your LINUX platform:

        sudo apt-get update

        sudo apt-get install -y make cmake git wget tar

-   Download the Microsoft Azure IoT Device SDK for C to your LINUX platform by issuing the following commands on the board:

        cd ~
        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Download cross compiler settings project to your LINUX platform by issuing the following commands on your LINUX platform:
        
        git clone https://github.com/sensplorer/spx_azure_iothub_c.git

-   Download and extract the Technexion gcc toolchain to your LINUX platform by issuing the following commands on your LINUX platform:
        
        wget -T 100 ftp://download.technexion.net/development_resources/development_tools/gcc/gcc-4.5.1-arm-codesourcery-2010.09-50.tar.xz
        tar -xf gcc-4.5.1-arm-codesourcery-2010.09-50.tar.xz

-   Set up the cross compiler settings by issuing the following commands on your LINUX platform:
        
         cp -a ~/spx_azure_iothub_c/dependencies/* ~/arm-2010.09/arm-none-linux-gnueabi/libc/
         cp -a ~/spx_azure_iothub_c/toolchain-spx-b1616.cmake ~/azure-iot-sdk-c/build_all/linux/

-   Edit the following file using any text editor of your choice:

    **For AMQP protocol:**

        ~/azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp.c

    **For HTTPS protocol:**

        ~/azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http.c

    **For MQTT protocol:**

        ~/azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Set up an environment containing the required toolchain root:

        cd ~/arm-2010.09/arm-none-linux-gnueabi/libc
        export SPX_ROOT=$(pwd)

    You can use *export -p* to verify SPX\_ROOT has been added to the environment.

-   Now we need to switch to the SDK directory tree. Enter this command:

        cd ~/azure-iot-sdk-c/build_all/linux/

-   Build the SDK using following command.

        ./build.sh --toolchain-file toolchain-spx-b1616.cmake -cl --sysroot=$SPX_ROOT

-   Copy samples from your LINUX platform to SENSPLORER SPx-B1616 Device:

    **For AMQP protocol:**

        scp ~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp root@<IP address of SENSPLORER SPx-B1616>:

    **For HTTPS protocol:**

        scp ~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http root@<IP address of SENSPLORER SPx-B1616>:

    **For MQTT protocol:**

        scp ~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt root@<IP address of SENSPLORER SPx-B1616>:
        

## 3.2 Send Device Events to IoT Hub:

-   Run the sample by issuing following command on SENSPLORER SPx-B1616 Device:

    **For AMQP protocol:**

        ./iothub_client_sample_amqp

    **For HTTPS protocol:**

        ./iothub_client_sample_http

    **For MQTT protocol:**

        ./iothub_client_sample_mqtt

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
