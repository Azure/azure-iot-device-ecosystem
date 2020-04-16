---
platform: rtos
device: esp32
language: c
---

Run a simple C sample on esp32 device running RTOS
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

This document describes how to connect esp32 device running RTOS with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Ubuntu 14.04
-   <https://github.com/Azure/azure-iot-sdk-c.git>
-   <https://www.espressif.com/en/products/hardware/esp32/overview>
-   esp32 device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Click [Here](https://docs.espressif.com/projects/esp-idf/en/latest/)

# Step 3: Build and Run the sample

## 3.1 Load the Azure IoT bits and prerequisites on device

-   Open a PuTTY session and connect to the device.

-   Install the prerequisite packages by issuing the following commands from the command line on the device. Choose your commands based on the OS running on your device.

    **Debian or Ubuntu**

        sudo apt-get update

        sudo apt-get install gcc git wget make libncurses-dev flex bison gperf python python-pip python-setuptools python-serial

    **ESP32 toolchain**
     
    ESP32 toolchain for Linux is available for download from Espressif website:

    For 64-bit Linux:

        https://dl.espressif.com/dl/xtensa-esp32-elf-linux64-1.22.0-80-g6c4433a-5.2.0.tar.gz
        mkdir -p ~/esp
        cd ~/esp
        tar -xzf ~/Downloads/xtensa-esp32-elf-linux64-1.22.0-80-g6c4433a-5.2.0.tar.gz
        export PATH="$PATH:$HOME/esp/xtensa-esp32-elf/bin"

    **Get ESP-IDF**

        cd ~/esp
        git clone --recursive https://github.com/espressif/esp-idf.git
        export IDF_PATH="/home/user-name/esp/esp-idf"

    **Install the Required Python Packages**
        
        python -m pip install --user -r $IDF_PATH/requirements.txt
    
    **Downloadthe SDK**
        
        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git
        
    **Porting code**
        
    Click [Here](https://github.com/dengjunyong/esp32_azure_iothub/blob/master/components/iothub_client/component.mk) to create component.mk

    **Code tree**

    Click [Here](https://github.com/dengjunyong/esp32_azure_iothub/tree/master/components/iothub_client) to find the code tree.

## 3.2 Build the samples

There is sample for sending and receiving messages to IoT Hub. 
    
### 3.2.1 Telemetry to IoT Hub Sample:

-   Click [Here](https://github.com/dengjunyong/esp32_azure_iothub/blob/master/main/iothub_client_sample_mqtt_esp8266.c) to find the **Main Code**

## 3.3 Run and Validate the Samples

-   Run the following command to compile and burn

        make
        make flash   

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

GitHub code address  
<https://github.com/dengjunyong/esp32_azure_iothub>
