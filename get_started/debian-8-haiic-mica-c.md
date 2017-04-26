---
platform: debian 8
device: haiic mica
language: c
---

Run a simple C sample on HAIIC MICA device running Debian 8 container
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Tips](#tips)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect HAIIC MICA device running Debian 8 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   HAIIC MICA device.
-   POE capable switch with web access.
-   M12 X-coded Ethernet cable for POE, See also [MICA Hardware Develompent Guide](http://www.harting-mica.com/fileadmin/harting/documents/lg/hartingtechnologygroup/landingpages/MICA/software/august2016/Dokumentation/Hardware_Development_Guide_V1.2.0.pdf).

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-   Follow the instructions given in [MICA Starting Up](http://www.harting-mica.com/fileadmin/harting/documents/lg/hartingtechnologygroup/landingpages/MICA/dokumentation/MICA_Inbetriebnahme_Starting_up_v2.pdf) and [MICA Admin Guide](http://www.harting-mica.com/fileadmin/harting/documents/lg/hartingtechnologygroup/landingpages/MICA/software/December2016/Administrator_Guide_v1.3.0.pdf) to set up your MICA device and login to MICA web GUI via browser.   
 -   When prompted, log in as admin with default password **admin** (it is recommended to change default password to password of your choice).
 -   Install Debian Container from [here](http://www.harting-mica.com/downloads/) and set up network settings for the container. See also [MICA Admin Guide](http://www.harting-mica.com/fileadmin/harting/documents/lg/hartingtechnologygroup/landingpages/MICA/software/December2016/Administrator_Guide_v1.3.0.pdf).
 -   Connect to Debian container via SSH
 -   Connection settings:
     -   Port = 22
     -   Connection Type = SSH
-   Log in as root. When prompted, type in root as password.
-   Make sure you got web access from within the container.

    ***Note:*** *If you run into problems, you might need to set default gateway for the container (most likely your router IP address) and nameserver in /etc/resolv.conf like following:*

        nameserver <your nameserver / probably your router IP>


<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

-   Open a PuTTY session and connect to the device.

-   Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by issuing the following commands from the command line on your board:

        sudo apt-get update

        sudo apt-get install -y curl libcurl4-openssl-dev uuid-dev uuid g++ make cmake git unzip openjdk-7-jre

-   Download the Microsoft Azure IoT Device SDK for C to the board by issuing the following command on the board::

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Edit the following file using any text editor of your choice:

        azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Build the SDK using following command.

        sudo ./azure-iot-sdk-c/build_all/linux/build.sh

## 3.2 Send Device Events to IoT Hub:

-   Run the sample by issuing following command:

        ~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

<a name="tips"></a>
# Tips

-   If you just want to build the serializer samples, run the following commands:

        cd ./c/serializer/build/linux
        make -f makefile.linux all


[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
