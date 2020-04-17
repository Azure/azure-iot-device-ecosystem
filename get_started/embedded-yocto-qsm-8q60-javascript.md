---
platform: embedded yocto 2.0
device: qsm-8q60
language: javascript
---

Run a simple JavaScript sample on QSM-8Q60 device running Embedded Yocto 2.0
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

This document describes how to connect QSM-8Q60 device running Embedded Yocto 2.0 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Prepare QSM-8Q60 Device][setup-hardware]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Empty microSD card 4GB at least.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   You should have following steps to prepare the device.

## 2.1 Install Yocto 2.0 into Linux Host PC

-   [Here][setup-yocto] is instruction how to install Yocto 2.0 and setup Linux Host PC.

## 2.2 Create projects and build images

-   [Here][setup-QSM-8Q60-yocto] is instruction to create projects and build images.

## 2.3 Customize root file system

-   Use the syntax below and add features to your **conf/local.conf** file.

        CORE_IMAGE_EXTRA_INSTALL += "\
        autoconf automake cpp cpp-symlinks gcc gcc-symlinks g++ g++-symlinks gettext make libstdc++ libstdc++-dev file coreutils libdaemon glibc glibc-dev python python-dev python-compiler python-numpy python-paho-mqtt cmake cmake-dev git curl curl-dev libcurl pkgconfig boost boost-dev valgrind valgrind-dev libcrypto libssl openssl openssl-conf openssl-dev uuid ossp-uuid-dev python-zlib util-linux-libuuid zlib-dev  python-requests python-pip"
        EXTRA_IMAGE_FEATURES = "debug-tweaks tools-debug tools-sdk dev-pkgs"
        IMAGE_INSTALL_append = " strace cmake boost"

-   Note that the space must be included before the first word in IMAGE_INSTALL_append and PACKAGECONFIG_pn_boost section. Then build new image by following command.

        bitbake  via-image-gui

-   New image will be created in **via-release-bsp/build-qsm8q60/tmp/deploy/images/imx6qsm8q60/
FirmwareInstall/image** directory. You can find the Root file system like below:

        rootfs.tgz

## 2.4 Create microSD card

-   Extract the QSM-8Q60-Linux-EVK-v3.0.2-20180306 file from EVK folder.
-   Next, copy the new image from via-release-bsp/build-qsm8q60/tmp/deploy/images/imx6qsm8q60/ FirmwareInstall/image folder to /sd_installer to replace the original image folder.
-   Using below command to flash image into SD card.

        sudo ./mk_sd_installer.sh /dev/<device name> --yocto

## 2.5 boot the OS from microSD card

-   Turn on the board and messages are shown through console over Debug Port.
-   Open and configure Putty or serial console terminal then check messages.

        Freescale i.MX Release Distro 4.1.15-1.1.1 imx6qvab820 /dev/ttymxc1
        imx6qvab820 login: root

-   You can login by **root** account

## 2.6 Add SSL certification path

-   Open and configure Putty or serial console terminal then check messages.

        export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
        export SSL_CERT_DIR=/etc/ssl/certs

## 2.7 Confirm system date is set to UTC timezone

-   Open and configure Putty or serial console terminal then check messages.

        date -utc

<a name="Build"></a>
# Step 3: Build and Run the Sample

## 3.1 Load the Azure IoT bits and prerequisites on device

-   Open a PuTTY session and connect to the device.

-   Download the SDK to the board by issuing the following command in PuTTY:

        git clone --recursive https://github.com/Azure/azure-iot-sdk-node.git

-   Verify that you now have a copy of the source code under the directory **azure-iot-sdk-node**.

### 3.1.1  Install node.js/npm and set up environment variables

-   Download Node.js and npm from here:

        https://nodejs.org/dist/v8.12.0/node-v8.12.0-linux-armv7l.tar.xz

-   Untar node-v8.12.0-linux-armv7l.tar.xz to folder:

        tar xvf node-v8.12.0-linux-armv7l.tar.xz

-   Update the PATH environment variable to include the full path to the bin folder containing Node. To ensure the correct path of Node and Npm run below command:     
       
        which node
        
3.  Ensure that the directory shown by the `which node` command matches one of the directories shown in your $PATH variable. You can confirm this by running following command.

        echo $PATH

4.  If node path is missing in PATH environment variable, run following command to set the same.    

        export PATH=[PathToNode]/bin:$PATH       

    **Note:** To test successful installation of Node JS and npm runtime, try to fetch its version information by running following command:

        node --version
        npm --version

<a name="BuildSamples"></a>
## 3.2 Build the samples

-   To validate the source code run the following commands on the device.

        export IOTHUB_CONNECTION_STRING='<iothub_connection_string>'

    Replace the `<iothub_connection_string>` placeholder with IoTHub Connection String you got in [Step 1](#Prerequisites).    

-   Run the following commands 

        cd ~/azure-iot-sdk-node
        cd build/Tools
        npm install

    ***Note:*** *Check all modules are download at node_modules folder.*

-   Install npm package to run sample.

        cd ~/azure-iot-sdk-node/device/samples

    **Install package for AMQP/HTTP/MQTT Protocol:**
	
        npm install

-   To update sample, run the following command on device.

        cd ~/azure-iot-sdk-node/device/samples
        vi simple_sample_device.js

-   This launches a console-based text editor. Scroll down to the protocol information.
    
-   Find the below code:

        var Protocol = require('azure-iot-device-amqp').Amqp;
        #var Protocol = require('azure-iot-device-amqp').Mqtt;
        #var Protocol = require('azure-iot-device-amqp').http;

    The default protocol used is AMQP. Code for other protocols(HTTP/MQTT) are mentioned just below the above line in the script.
    Uncomment the line as per the protocol you want to use.

-   Scroll down to the connection information.
    
    Find the following place holder for IoT connection string:

        var connectionString = "[IoT Device Connection String]";

-   Replace the above placeholder with device connection string. You can get this from DeviceExplorer as explained in [Step 1](#Prerequisites), that you copied into Notepad.

-   Save your changes and exit vi.

-   Run the following command before leaving the **~/azure-iot-sdk-node/device/samples** directory

        npm link azure-iot-device

<a name="Run"></a>
## 3.3 Run and Validate the Samples

### 3.3.1 Send Device Events to IOT Hub

-   Run the sample by issuing following command and verify that data has been successfully sent and received.

        node ~/azure-iot-sdk-node/device/samples/simple_sample_device.js

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

### 3.3.2 Receive messages from IoT Hub

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
[setup-hardware]: http://cdn.viaembedded.com/products/docs/qsm-8q60/user-manual/UM_QSM-8Q60_v1.04_180518.pdf
[setup-QSM-8Q60-yocto]: http://cdn.viaembedded.com/products/docs/qsm-8q60/Linux_Development_Guide/QSM-8Q60_Linux_BSP_v3.0.2_Development_Guide_v1.00_20180305.pdf
[setup-yocto]: http://cdn.viaembedded.com/products/docs/qsm-8q60/Linux_Development_Guide/QSM-8Q60_Linux_BSP_v3.0.2_Development_Guide_v1.00_20180305.pdf