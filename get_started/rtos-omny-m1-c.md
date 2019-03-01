---
platform: rtos
device: omny-m1
language: c
---

Run a simple C sample on OMNY-M1 device running RTOS
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

# Introduction

**About this document**

This document describes how to connect OMNY-M1 device running RTOS with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   An Ubuntu environment should be set up to build firmware
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   OMNY-M1 device

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

## 2.1 Azure IoT Hub

-   [Get iothub connection string (primary key)](https://azure.microsoft.com/en-in/services/iot-hub/) from the Azure IoT Hub, which will be used later. An example can be seen below:

        HostName=yourname-ms-lot-hub.azure-devices.cn;SharedAccessKeyName=iothubowner;SharedAccessKey=zMeLQ0JTlZXVcHBVOwRFVmlFtcCz+CtbDpUPBWexbIY=

-   For step-by-step instructions, please click [here](https://github.com/espressif/esp-azure/blob/master/doc/IoT_Suite.md).

## 2.2. Azure CLI

-   Install [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

After that, you should be able to use azure CLI to manage your iot-device.

## 2.3. Device Connection String

-   Login to Azure CLI
-   create your device, and get a **device connection string**. An example can be seen:

        "HostName=esp-hub.azure-devices.net;DeviceId=yourdevice;SharedAccessKey=L7tvFTjFuVTQHtggEtv3rp+tKEJzQLLpDnO0edVGKCg=";

For detailed instruction, please click [Here](https://github.com/espressif/esp-azure/blob/master/doc/azure_cli_iot_hub.md).
 
## 2.4. SDK

-   [AZURE-SDK](https://github.com/espressif/esp-azure) can be used to connect your OMNY-M1 devices to Azure, using MQTT protocol.
-   Espressif SDK [ESP-IDF](https://github.com/espressif/esp-idf) to be used to compile FreeRTOS based firmware for OMNY-M1 devices.


# Step 3: Build and Run the sample

## 3.1 Load the Azure IoT bits and prerequisites on development environment

-   Login to your development environment such as Ubuntu-16.04

-   Install the prerequisite packages by issuing the following commands from the command line on the development environment. Choose your commands based on the OS running on your device.

    **Debian or Ubuntu**

        sudo apt-get update

        sudo apt-get install gcc git wget make libncurses-dev flex bison gperf python python-pip python-setuptools python-serial

    **ESP32 toolchain**
     
    ESP32 toolchain for Linux is available for download from Espressif website:

    For 64-bit Linux:

        https://dl.espressif.com/dl/xtensa-esp32-elf-linux64-1.22.0-80-g6c4433a-5.2.0.tar.gz
        mkdir -p ~/omny-m1
        cd ~/omny-m1
        tar -xzf ~/Downloads/xtensa-esp32-elf-linux64-1.22.0-80-g6c4433a-5.2.0.tar.gz
        export PATH="$PATH:$HOME/omny-m1/xtensa-esp32-elf/bin"

    **Get ESP-IDF**

        cd ~/omny-m1
        git clone --recursive https://github.com/espressif/esp-idf.git
        export IDF_PATH="/home/user-name/omny-m1/esp-idf"

    **Install the Required Python Packages**
        
        python -m pip install --user -r $IDF_PATH/requirements.txt
    
    **Download ESP Azure IoT SDK**

        cd ~/omny-m1
        git clone --recursive https://github.com/espressif/esp-azure.git

## 3.2 Build the samples

There is integrated sample for sending and receiving messages for Azure IoT Hub end. 

### 3.2.1 Cloning Git submodules

        cd ~/omny-m1/esp-azure
        git submodule update --init --recursive

### 3.2.2 Configure WiFi and Azure-IoT for OMNY-M1:

        make menuconfig
		Example Configuration  --->
		(WiFi-Router-SSID) WiFi SSID
		(WiFi-Passkey) WiFi Password
		(Azure-Device-Connection-String) IOT Hub Device Connection String

- Note : Set respective values in above configuration settings as per your WiFi Router setting and Azure Device credentials

## 3.2.3 Configuring and Building

-   Run the following command to compile and burn on OMNY-M1

        make
        make flash

## 3.3 Run and Validate the Samples

-   Execute and monitor sample app. on OMNY-M1

        make monitor

-   [Monitor device-to-cloud events](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/readme.md#monitor)
-   [Send cloud-to-device messages](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/readme.md#send)

	
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

