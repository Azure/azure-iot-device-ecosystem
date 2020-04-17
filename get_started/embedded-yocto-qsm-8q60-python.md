---
platform: embedded yocto 2.0
device: qsm-8q60
language: python
---

Run a simple PYTHON sample on QSM-8Q60 device running Embedded Yocto 2.0
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
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Load the Azure IoT bits and prerequisites on device

-   Open a PuTTY session and connect to the device.

-   Download the SDK to the board by issuing the following command in PuTTY:

        git clone --recursive https://github.com/Azure/azure-iot-sdk-python.git

-   Verify that you now have a copy of the source code under the directory **azure-iot-sdk-python**.

## 3.2 Build SDK and sample

-   Open a PuTTY session and connect to the device.

-   You can verify that the environment variables necessary to run python have been set correctly by running

        python --version

-   Download netifaces module for python:

        https://files.pythonhosted.org/packages/81/39/4e9a026265ba944ddf1fea176dbb29e0fe50c43717ba4fcf3646d099fe38/netifaces-0.10.7.tar.gz

-   Run following commands to install module:

        tar xvf netifaces-0.10.7.tar.gz
        cd netifaces-0.10.7.tar.gz
	    python install setup.py

-   Run following commands to build the SDK:

        cd python/build_all/linux
	    sudo ./build.sh | tee BuildPython_LogFile.txt

-   After a successful build, the `iothub_client.so` Python extension module is copied to the **python/device/samples** folder.

-   Navigate to samples folder by executing following command:

        cd azure-iot-sdk-python/device/samples/

-   Edit the following file using any text editor of your choice:

    **For AMQP protocol:**

        vi iothub_client_sample_amqp.py

    **For HTTP protocol:**

        vi iothub_client_sample_http.py

    **For MQTT protocol:**

        vi iothub_client_sample_mqtt.py

-   Find the following place holder for device connection string:

        connectionString = "[device connection string]"

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

## 3.3 Send Device Events to IoT Hub:

-   Run the sample application using the following command:

    **For AMQP protocol:**

        python iothub_client_sample_amqp.py

    **For HTTP protocol:**

        python iothub_client_sample_http.py

    **For MQTT protocol:**

        python iothub_client_sample_mqtt.py

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.4 Receive messages from IoT Hub

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