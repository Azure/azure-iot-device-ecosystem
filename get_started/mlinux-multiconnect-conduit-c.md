---
platform: mlinux
device: multiconnect conduit
language: c
---

Run a simple C sample on MultiConnect Conduit running mLinux
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Tips](#tips)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect MultiConnect Conduit device running mLinux with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   MultiConnect Conduit device.
-   A host computer or virtual machine running Linux with a connection to your MultiConnect Conduit
-   [mLinux C/C++ Toolchain][lnk-mlinux-toolchain]

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

Follow the appropriate Getting Started guide for your Conduit to connect to the same network as your host PC
-   [Getting Started with AEP][lnk-aep]
-   [Getting Started with mLinux][lnk-mlinux]

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

-   If you have not already done so, install the mLinux C/C++ Toolchain: <http://www.multitech.net/developer/software/mlinux/mlinux-software-development/mlinux-c-toolchain/>
-   From a terminal on the host, load the cross compilation environment using:

        $ source /<path to sdk>/environment-setup-arm926ejste-mlinux-linux-gnueabi
	
    By default this would be::
		
        $ source /opt/mlinux/<version>/environment-setup-arm926ejste-mlinux-linux-gnueabi
		
-   Download the Microsoft Azure IoT Device SDK for C to the board by issuing the following command on the host

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   The MultiConnect Conduit supports AMQP, HTTP and MQTT.  Edit the example file for the protocol(s) you wish to use using any text editor of your choice:

    -   AMQP

            azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp.c

    -   HTTP

            azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_http.c
		
    -    MQTT

            azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_mqtt.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Build the SDK using following command.

        ./azure-iot-sdk-c/build_all/linux/build.sh
		
-   Transfer the compiled example(s) to your MultiConnect Conduit:

        scp ./cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp admin@<MultiConnect Conduit IP address>:~/
		scp ./cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http admin@<MultiConnect Conduit IP address>:~/
		scp ./cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt admin@<MultiConnect Conduit IP address>:~/

## 3.2 Send Device Events to IoT Hub:

-   Open a terminal connection to your MultiConnect Conduit over ssh or microUSB
 
-   Run the sample by issuing the command for the protocol you wish to run:
        
		~/iothub_client_sample_amqp
		~/iothub_client_sample_http
		~/iothub_client_sample_mqtt

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

<a name="tips"></a>
# Tips

-   If you just want to build the serializer samples, run the following commands:

    ```
    cd ./c/serializer/build/linux
    make -f makefile.linux all
    ```

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
[lnk-mlinux-toolchain]: Toolchain]:http://www.multitech.net/developer/software/mlinux/mlinux-software-development/mlinux-c-toolchain/
[lnk-aep]: http://www.multitech.net/developer/software/aep/getting-started-aep/
[lnk-mlinux]: http://www.multitech.net/developer/software/mlinux/getting-started-with-conduit-mlinux/
