---
platform: yocto
device: zk1050
language: c
---

Run a simple C sample on ZK1050 device running Yocto
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

The following document describes the process of connecting ZK1050 to Azure IoT Hub.
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
The process requires to prepare a cross compile environment. In the case to use Ubuntu 14.04 Linux is ideally.
-   Install required packages on Ubuntu
  
        sudo apt-get install -y gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat libsdl1.2-dev xterm sed cvs subversion coreutils texi2html docbook-utils python-pysqlite2 help2man make gcc g++ desktop-file-utils libgl1-mesa-dev libglu1-mesa-dev mercurial autoconf automake groff curl lzop asciidoc u-boot-tools
    
-   [To build Yocto from source code](https://www.yoctoproject.org/docs/current/brief-yoctoprojectqs/brief-yoctoprojectqs.html "Build Yocto from source code")Please find NXP I.MX6 Version : imx-4.1.15-2.1.0(imx-4.1-krogoth)
-   [Install toolchain on Ubuntu](http://variwiki.com/index.php?title=Yocto_Toolchain_installation&release=RELEASE_MORTY_BETA_DART-6UL#Prerequirements "Install toolchain on ubuntu")
-   [Setup your IoT hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

<a name="Step-2-PrepareDevice"></a>
# Step 2: Prepare your Device

-   Ready ZK1050
-   Ensure ZK1050 with Internet access via ethernet port

<a name="Step-3-Build"></a>
# Step 3: Build and Run the sample

### Download the Microsoft Azure Iot Hub C SDK 

The default installation of git does not support submodules, but if you install a submodule enabled version of git, or build git from source, you may use Git and clone the Azure SDK repository directly. Please use the following commands:

    $ git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

We will build a sample application which relies on the SDK. We first need to update the credentials in the sample AMPQ app to match those of our Azure IoT Hub application. When we build the Azure IoT SDK, the sample C applications are automatically built by default, we need to include our credentials into the sample app while we build the SDK so that they are ready to function after we build.

-   Edit the following file using any text editor of your choice:
      For AMQP protocol:

        $ azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp.c

-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]"; 

- Replace the above placeholder with device connection string

### Build and Run the sample

-   [Build the sample code by Yocto toolchain ](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/SDK_cross_compile_example.md "Build the SDK by Yocto toolchain ")

-   Copy the AMQP sample to device:

        ~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp

-   run the sample

### Receive messages from IoT Hub

See [Manage IoT Hub](http://https://catalog.azureiotsolutions.com/docs?title=Azure/azure-iot-device-ecosystem/manage_iot_hub "Manage IoT Hub") to learn how to send cloud-to-device messages to the application.

<a name="NextSteps"></a>
# Step 4: Next steps

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