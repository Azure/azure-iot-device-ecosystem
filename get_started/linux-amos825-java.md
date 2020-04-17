---
platform: embedded yocto 1.7
device: amos825
language: java
---

Run a simple Java sample on AMOS825 running Yocto 1.7 
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

This document describes how to build AMOS825 image and running Yocto 1.7 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Sign Up To Azure IoT Hub][sign-iot-hub]
-   [Prepare AMOS825][setup-hardware]
-   [Prepare your development environment][setup-AMOS825-yocto]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Empty microSD card 4GB at least.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

You should have following steps to prepare the device.

## 2.1 Intall Yocto1.7 into Linux Host PC

-   [Here][setup-yocto] is instruction how to install Yocto 1.7 and setup Linux Host PC.

## 2.2 Create projects and build images

-   [Here][setup-AMOS825-yocto] is instruction to create projects and build images.
-   Or you could download test image from [here][demo-image] without rebuild OS image then pass 2.3 step.

## 2.3 Customize root file system

-   Please add meta-java under sources folder.

        cd via-release-bsp/sources/
        git clone git://git.yoctoproject.org/meta-java

-   Modify file jamvm.inc and jikes_1.22.bb.

        meta-java/recipes-core/jamvm/jamvm.inc
		meta-java/recipes-core/jikes/jikes_1.22.bb
		---------------------------------------------------------------
        ---------------------------------------------------------------
		#inherit autotools update-alternatives relative_symlinks
		inherit autotools update-alternatives

-   Add meta-java path in your **conf/bblayers.conf** file.

        cd via-release-bsp/build-vab820/conf
		gedit bblayers.conf
		---------------------------------------------------------------
		---------------------------------------------------------------
		#added by hob
		BBLAYERS = ".....${BSPDIR}/sources/meta-via-bsp/meta-via-arm ${BSPDIR}/sources/meta-java"

-   Use the syntax below and add features to your **conf/local.conf** file.

        CORE_IMAGE_EXTRA_INSTALL += "\
		autoconf automake cpp cpp-symlinks gcc gcc-symlinks g++ g++-symlinks gettext make libstdc++ libstdc++-dev file coreutils libdaemon glibc glibc-dev python python-dev python-compiler python-numpy python-paho-mqtt cmake cmake-dev git curl curl-dev libcurl pkgconfig boost boost-dev valgrind valgrind-dev libcrypto libssl openssl openssl-conf openssl-dev uuid ossp-uuid-dev python-zlib util-linux-libuuid zlib-dev openjdk-8"

-   Note that the space must be included before the first word in IMAGE_INSTALL_append and PACKAGECONFIG_pn_boost section. Then build new image by following command.

        bitake via-image-gui

-   New image will be created in **tmp/deploy/images/imx6qvab820** directory.
-   Specifically, you can find an file like below:

        via-image-gui-imx6qvab820-{date}.rootfs.sdcard


## 2.4 Create microSD card

-   umount your 4GB size SD card.
-   Using below command to flash image into SD card.

        sudo dd if=via-image-gui-imx6qvab820-{date}.rootfs.sdcard of=/dev/sdx bs=1M conv=fsync


## 2.5 boot the OS from microSD card

-   Turn on the board and messages are shown through console over Debug Port.
-   Open and configure Putty or serial console terminal then check messages.

        Freescale i.MX Release Distro 4.1.15-1.1.1 imx6qvab820 /dev/ttymxc1

        imx6qvab820 login:

-   You can login by root account
-   Download maven and set environment

	    wget http://ftp.twaren.net/Unix/Web/apache/maven/maven-3/3.5.0/binaries/apache-maven-3.5.0-bin.tar.gz
    	tar zxvf apache-maven-3.5.0-bin.tar.gz
    	mv apache-maven-3.5.0 maven
    	export PATH=$PATH:$MAVEN_HOME/bin

## 2.6 Confirm software version

Check software version. **cmake** version is 2.8.1. **gcc** version is 4.9.0. And **java** is 1.7.

        cmake --version
        gcc --version
        java --version


<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Install SDK

-   Prepare an x86 PC with Ubuntu 14.04 or higher for Java compile

		sudo apt-get install openjdk-7-jdk maven git
   
-   Download Azure IoT Device SDK for Java by following commands.

        git clone https://github.com/Azure/azure-iot-sdk-java.git

## 3.2 Build SDK and sample

-   Run following commands to build the SDK:

        cd azure-iot-sdk-java/device
        mvn install   

-   After a successful build, you will find *.jar file like below.

		azure-iot-sdk-java/device/iothub-java-client/target/iothub-java-client-{version}-with-deps.jar

- Copy below files into Yocto 1.7 by executing following command:

        azure-iot-sdk-java/device/iot-device-samples/send-event/target/send-event-{version}-with-deps.jar
		azure-iot-sdk-java/device/iot-device-samples/handle-messages/target/handle-messages-{version}-with-deps.jar

## 3.3 Send Device Events to IoT Hub:

-   Run the sample application using the following command:

    **For AMQP protocol:**

        ava -jar ./send-event-{version}-with-deps.jar "{connection string}" "{number of requests to send}" "amqps"

    **For HTTP protocol:**

        java -jar ./send-event-{version}-with-deps.jar "{connection string}" "{number of requests to send}" "https"

    **For MQTT protocol:**

        java -jar ./send-event-{version}-with-deps.jar "{connection string}" "{number of requests to send}" "mqtt"

	**For Websocket with AMQP protocol:**

        java -jar ./send-event-{version}-with-deps.jar "{connection string}" "{number of requests to send}" "amqps_ws"


-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.4 Receive messages from IoT Hub

-   Run the sample application using the following command:

    **For AMQP protocol:**

        java -jar ./handle-messages-{version}-with-deps.jar "{connection string}" "amqps"

	**For HTTP protocol:**

        java -jar ./handle-messages-{version}-with-deps.jar "{connection string}" "https"

	**For MQTT protocol:**

        java -jar ./handle-messages-{version}-with-deps.jar "{connection string}" "mqtt"

	**For Websocket with AMQP protocol:**

        java -jar ./handle-messages-{version}-with-deps.jar "{connection string}" "amqps_ws"

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
[setup-hardware]: https://cdn.viaembedded.com/products/docs/amos-825/user_manual/UM_AMOS-825_v1.04_170417.pdf
[setup-AMOS825-yocto]: https://cdn.viaembedded.com/products/docs/amos-825/Linux_image_installation_guide/AMOS-825_Linux_EVK_v3.0.2_Image_Installation_Guide_v1.00_20170112.pdf
[sign-iot-hub]: https://account.windowsazure.com/signup?offer=ms-azr-0044p
[setup-yocto]: https://cdn.viaembedded.com/products/docs/amos-825/Linux_image_installation_guide/AMOS-825_Linux_EVK_v3.0.2_Image_Installation_Guide_v1.00_20170112.pdf
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[demo-image]: https://drive.google.com/file/d/0BxMsYodt6EeOVElwY0xRVFQwTUE/view?usp=sharing