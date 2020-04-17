---
platform: qualcomm linux
device: tc500
language: c
---

Run a simple C sample on TC500 device running Qualcomm Linux
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

This document describes how to connect TC500 device running Qualcomm Linux with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   TC500 device.
-   PC to Build cross compile.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Install Ubuntu on your development PC or VM.
-   Install cross-compiler of TC500 on development PC or VM.
-   Install cross-compiled openssl(1.1.1), curl(7.54.0-DEV) libs and header files on development PC or VM.

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Prepare SDK Build

-   Download the Microsoft Azure IoT Device SDK for C to your LINUX development VM by issuing the following command:

         git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Set up the cross compiler setting. Get the Sourcery G++ Lite package from the CodeSourcery [download site](https://sourcery.mentor.com/sgpp/lite/arm/portal/subscription?@template=lite).

         export TC500_TOOLCHAIN_ROOT=[Your Toolchain Root]
         export OPENSSL_ARM_ROOT=[Your cross-compiled OPENSSL Library install Root]
         export CURL_ARM_ROOT=[Your cross-compiled CULR Library install Root]

-   Create the following file (this file is necessary to cross build the SDK):

         azure-iot-sdk-c/build_all/linux/toolchain-tc500.cmake

    -   Insert the following lines to the file: "toolchain-tc500.cmake"

            INCLUDE(CMakeForceCompiler)

            SET(CMAKE_SYSTEM_NAME Linux)    # this one is important
            SET(CMAKE_SYSTEM_VERSION 1)     # this one not so much

            # this is the location of toolchain targeting the TC500
            SET(CMAKE_C_COMPILER $ENV{TC500_TOOLCHAIN_ROOT}/bin/arm-none-linux-gnueabi-gcc)

            # this is the file system root of the target
            SET(OPENSSL_ROOT_DIR "$ENV{OPENSSL_ARM_ROOT}")
            SET(OPENSSL_INCLUDE_DIR "$ENV{OPENSSL_ARM_ROOT}/include")
            SET(OPENSSL_LIBRARIES "$ENV{OPENSSL_ARM_ROOT}/lib/libssl.a")

            SET(CURL_LIBRARY "$ENV{CURL_ARM_ROOT}/lib/libcurl.a")
            SET(CURL_INCLUDE_DIR "$ENV{CURL_ARM_ROOT}/include/")

            # search for programs in the build host directories
            SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)

            # for libraries and headers in the target directories
            #SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
            #SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
            SET(ENABLE_EXPORTS ON)

            link_directories($ENV{CURL_ARM_ROOT}/lib/)
            link_directories($ENV{OPENSSL_ARM_ROOT}/lib/)

-   Edit the following file (this file is necessary to cross build the SDK):

        azure-iot-sdk-c/build_all/linux/build.sh

    -   Insert cmake rule the following lines to the file: "build.sh"

          # Do not use uuid library
          -Duse_default_uuid=ON

-   Edit the following file (this file is necessary to cross build the SDK):

        azure-iot-sdk-c/CMakeLists.txt

    -   Make warning as error Rules Remove

            # set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Werror")
            set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
            # set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Werror")
            set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}")

## 3.2 Build SDK and sample in TC500

-   Edit the following file using any text editor of your choice:

  **For HTTP protocol:**
  
  
    azure-iot-sdk-c\iothub_client\samples\iothub_client_sample_http\iothub_client_sample_http.c
		 
-   Find the following place holder for IoT connection string:

      static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Build the SDK using following command.

	  cd ./azure-iot-sdk-c/build_all/linux/
      ./build.sh --toolchain-file toolchain-tl500.cmake -cl --sysroot=$TC500_TOOLCHAIN_ROOT

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.
			
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

