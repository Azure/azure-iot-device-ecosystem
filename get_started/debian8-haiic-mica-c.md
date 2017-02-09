---
platform: HARTING MICA Linux 1.0 (Adlershof) / Debian Jessie container
device: HAIIC MICA
language: c
---

Run a simple C sample on HAIIC MICA device running Debian container
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Step-1-Prerequisites)
-   [Step 2: Prepare your Device](#Step-2-PrepareDevice)
-   [Step 3: Build and Run the Sample](#Step-3-Build)
-   [Tips](#tips)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect HAIIC MICA device with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Step-1-Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Computer.
-   MICA device.
-   SSH client on your desktop computer, such as [PuTTY](http://www.putty.org/), so you can remotely access the command line on the Debian container.
-   POE capable switch with web access.
-   M12 X-coded Ethernet cable for POE, see also [MICA Hardware Develompent Guide](http://www.harting-mica.com/fileadmin/harting/documents/lg/hartingtechnologygroup/landingpages/MICA/software/august2016/Dokumentation/Hardware_Development_Guide_V1.2.0.pdf)
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

<a name="Step-2-PrepareDevice"></a>
# Step 2: Prepare your Device
-   Follow the instructions given in [MICA Starting Up](http://www.harting-mica.com/fileadmin/harting/documents/lg/hartingtechnologygroup/landingpages/MICA/dokumentation/MICA_Inbetriebnahme_Starting_up_v2.pdf) and 
[MICA Admin Guide](http://www.harting-mica.com/fileadmin/harting/documents/lg/hartingtechnologygroup/landingpages/MICA/software/December2016/Administrator_Guide_v1.3.0.pdf) site to set up your MICA device and login to MICA web GUI via browser.   
-   When prompted, log in as admin with default password **admin** (it is recommended to change default password to password of your choice.)
-   Install Debian Container from [here](http://www.harting-mica.com/downloads/) and set up network settings for the container, see also [MICA Admin Guide](http://www.harting-mica.com/fileadmin/harting/documents/lg/hartingtechnologygroup/landingpages/MICA/software/December2016/Administrator_Guide_v1.3.0.pdf)
-   Connect to Debian container via SSH
-   Connection settings:
    -   Port = 22
    -   Connection Type = SSH
-   Log in as root, when prompted, type in root as password.
-   Make sure you got web access from within the container by running

        apt-get update
        
    ***Note:*** *If you run into problems, you might need to set default gateway for the container (most likely your router IP address) and nameserver in /etc/resolv.conf like following:*

        nameserver <your nameserver / probably your router IP>


<a name="Step-3-Build"></a>
# Step 3: Build and Run the sample

<a name="Step-3-1-Load"></a>
## 3.1 Build SDK and sample

-   Open a PuTTY session and connect to the device.

-   Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by issuing the following commands from the command line on your board:

        apt-get install -y curl libcurl4-openssl-dev build-essential cmake git uuid-dev

    ***Note:*** *This setup process requires cmake version 2.8.12 or higher.* 
    
    *You can verify the current version installed in your environment using the  following command:*

        cmake --version

    *This library also requires gcc version 4.9 or higher. You can verify the current version installed in your environment using the following command:*
    
        gcc --version 

-   Download the Microsoft Azure IoT Device SDK for C to the board by issuing the following command on the board::

        git clone --recursive https://github.com/Azure/azure-iot-sdks.git

-   Edit the following file using any text editor of your choice:

        azure-iot-sdks/c/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Step-1-Prerequisites) and save the changes.

-   Build the SDK samples using the following command:

        ./azure-iot-sdks/c/build_all/linux/build.sh | tee LogFile.txt

## 3.2 Send Device Events to IoT Hub

-   Run the sample by issuing following command:

        azure-iot-sdks/c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.
