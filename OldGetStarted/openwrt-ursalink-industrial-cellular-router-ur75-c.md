---
platform: openwrt
device: ursalink industrial cellular router ur75
language: c
---

Run a simple C sample on Ursalink Industrial Cellular Router UR75 device running Linux
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect Ursalink Ursalink Industrial Cellular Router UR75 device running OpenWRT with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Ursalink Industrial Cellular Router UR75 device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   The URL for the device is <http://www.ursalink.com/industrial-cellular-router-ur75/> Connect the Usalink Industrial Cellular Router UR7x using the SSH with putty.

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK on Ubuntu machine

### 3.1.1 Setup the development environment

    sudo apt-get update

    sudo apt-get install -y curl uuid-dev libcurl4-openssl-dev build-essential cmake git

### 3.1.2 Install the Cross Compilation Toolchain

    sudo tar -xzvf toolchain-aarch64_armv8-a_gcc-5.4.0_musl-1.1.16.tar.gz -C /opt
        
    export PATH=$PATH:/opt/staging_dir/toolchain-aarch64_armv8-a_gcc-5.4.0_musl-1.1.16/bin
       
### 3.1.3 Clone github repository

    git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git
        
### 3.1.4 cross compile azure iot sdk for c 

    cd azure-iot-sdk-c
    touch AZURE_IOT_UR7X_CROSS_COMPILE.cmake

The content is as follows:

    INCLUDE(CMakeForceCompiler)
    SET(UR7x_CROSS_COMPILE_PATH "/opt/staging_dir/toolchain-aarch64_armv8-a_gcc-5.4.0_musl-1.1.16")
    SET(UR7x_SYSTEM_PATH "/home/kenny/UROS/UR7x")
    SET(CMAKE_SYSTEM_NAME Linux)
    SET(CMAKE_SYSTEM_VERSION 1)
    SET(CMAKE_SYSROOT "${UR7x_SYSTEM_PATH}/usr")
    SET(CMAKE_C_COMPILER ${UR7x_CROSS_COMPILE_PATH}/bin/aarch64-openwrt-linux-gcc)
    SET(CMAKE_CXX_COMPILER ${UR7x_CROSS_COMPILE_PATH}/bin/aarch64-openwrt-linux-g++)

build project:

    ./build_all/linux/build.sh	--toolchain-file AZURE_IOT_UR7X_CROSS_COMPILE.cmake

## 3.2 Running the sample

### 3.2.1 Use "tftp" command to copy files from the sample's directory on Ubuntu to UR7X device

    tftp -gr ./cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp 192.168.2.166

    tftp -gr ./cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http 192.168.2.166

    tftp -gr ./cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt 192.168.2.166

    tftp -gr ./cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_amqp_websockets/iothub_client_sample_amqp_websockets 192.168.2.166

    tftp -gr ./cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_mqtt_websockets/iothub_client_sample_mqtt_websockets 192.168.2.166
         
### 3.2.2 Send Device Events to IoT Hub

#### Run the sample application using the following command

-   For AMQP protocol:

        ./iothub_client_sample_amqp
        
-   For HTTP protocol:

        ./iothub_client_sample_http
        
-   For MQTT protocol:

        ./iothub_client_sample_mqtt
        
-   For WebSocket with AMQP protocol

        ./iothub_client_sample_amqp_websockets
        
-   For WebSocket with MQTT protocol

        ./iothub_client_sample_mqtt_websockets
        
### 3.2.3 Receive messages from IoT Hub

See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer]
-   [Save IoT Hub messages to Azure data storage]
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub]
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]
-   [Remote monitoring and notifications with Logic Apps]   

[Manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[Save IoT Hub messages to Azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[Use Power BI to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[Remote monitoring and notifications with Logic Apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

