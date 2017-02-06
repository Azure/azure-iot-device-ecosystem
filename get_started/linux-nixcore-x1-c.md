---
platform: openwrt/lede
device: nixcore x1
language: c
---

Run a simple C sample on NixCore X1 device running OpenWRT/LEDE
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

This document describes how to connect NixCore X1 device running OpenWRT/LEDE with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   NixCore X1 device
-   Linux machine for building software
-   Connection to NixCore command line (Serial or SSH)

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-   Download OpenWRT/LEDE .config file from [NixCores website](http://nixcores.com)

-   Download and compile an OpenWRT/LEDE toolchain and Root File System (RFS).  Instructions on building OpenWRT/LEDE are provided in the [NixCore X1 Documentation NIXD01001](http://nixcores.com/products/nixcore-x1/#documentation) in section 7.4.1

-   Determine the path location of the staging dir, toolchain and RFS
    -   Example staging dir: /home/dev/nixcore/lede-source/staging_dir/
    -   Example toolchain path: /home/dev/nixcore/lede-source/staging_dir/toolchain-mipsel_24kc_gcc-5.4.0_musl-1.1.15/bin/
    -   Example RFS path: /home/dev/nixcore/lede-source/staging_dir/target-mipsel_24kc_musl-1.1.15/
    
-   Confirm the NixCore can access the internet: ping <domain\>

#### NOTES:
-   Development machine = Linux machine used to cross compile the Azure SDK

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

-   Download the Microsoft Azure IoT Device SDK for C to the development machine by issuing the following command:

        git clone --recursive https://github.com/Azure/azure-iot-sdks.git

-   Edit the following file using any text editor of your choice:

    **For AMQP protocol:**

        azure-iot-sdks/c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp.c

    **For MQTT protocol:**

        azure-iot-sdks/c/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt.c
        
    **For HTTPS protocol:**

        azure-iot-sdks/c/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Download the Azure cmake cross compile toolchain file (toolchain-nixcore.cmake) from the NixCores website [http://nixcores.com/azure/](http://nixcores.com/azure/).

-   Store the Toolchain file in ./azure-iot-sdks/c/ folder

-   Edit the Toolchain file, set the following variables you gathered from [Step 2](#PrepareDevice)
    
        CMAKE_C_COMPILER = <openwrt_toolchain_path\>/mipsel-openwrt-linux-gcc
        CMAKE_CXX_COMPILER = <openwrt_toolchain_path\>/mipsel-openwrt-linux-g++
        CMAKE_FIND_ROOT_PATH = <openwrt_RFS_path\>

-   Set the STAGING_DIR environment variable for the OpenWRT/LEDE compiler from [Step 2](#PrepareDevice)

        export STAGING_DIR=<openwrt_staging_dir>

-   Build the SDK using following command, replacing the RFS path with the location you gathered in [Step 2](#PrepareDevice)

        sudo ./azure-iot-sdks/c/build_all/linux/build.sh -cl --sysroot=<openwrt_RFS_path> --toolchain-file ./azure-iot-sdks/c/toolchain-nixcore.cmake

## 3.2 Send Device Events to IoT Hub:

-   Copy the sample binaries to the NixCore X1 root home directory via scp:

    **If using HTTP protocol:**
    
        scp azure-iot-sdks/c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http

        root@<nixcore_ip>:~

    **If using MQTT protocol:**
    
        scp azure-iot-sdks/c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt

        root@<nixcore_ip>:~
        
    **If using AMQP protocol:**

        scp azure-iot-sdks/c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp

        root@<nixcore_ip>:~
    
-   From the NixCore command line, execute tests

    **If using AMQP protocol:**

        ~/iothub_client_sample_amqp

    **If using HTTP protocol:**

        ~/iothub_client_sample_http

    **If using MQTT protocol:**

        ~/iothub_client_sample_mqtt

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

<a name="tips"></a>
# Tips

-   If libuuid is not found on the NixCore, it can be found in the RFS path and copied to the NixCore

-   On Development Machine:

        scp <openwrt_RFS_path>/usr/lib/libuuid.so root@<nixcore_ip>:/usr/lib/
        
-   On NixCore:

        cd /usr/lib/
        ln -s libuuid.so libuuid.so.1
        

[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
