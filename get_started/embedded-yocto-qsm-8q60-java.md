---
platform: embedded yocto 2.0
device: qsm-8q60
language: java
---

Run a simple JAVA sample on QSM-8Q60 device running Embedded Yocto 2.0
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

## 3.1 Load the Azure IoT bits and prerequisites on device

-   Open a PuTTY session and connect to the device.

-   Download the SDK to the board by issuing the following command in PuTTY:

        git clone https://github.com/Azure/azure-iot-sdk-java.git

-   Verify that you now have a copy of the source code under the directory **azure-iot-sdk-java**.

### 3.1.1  Install Java JDK and set up environment variables

-   Download Java JDK 8.0 from here:

        http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jdk-8u181-linux-arm32-vfp-hflt.tar.gz

-   Untar jdk-8u181-linux-arm32-vfp-hflt.tar.gz to folder:

        tar xvf jdk-8u181-linux-arm32-vfp-hflt.tar.gz

-   Update the PATH environment variable to include the full path to the bin folder containing Java. To ensure the correct path of Java run below command:     
       
        which java
        
3.  Ensure that the directory shown by the `which java` command matches one of the directories shown in your $PATH variable. You can confirm this by running following command.

        echo $PATH

4.  If Java path is missing in PATH environment variable, run following command to set the same.    

        export PATH=[PathToJava]/bin:$PATH       

    ***NOTE:*** *Here **[PathToJava]** is output of `which java` command. For example, if `which java` output is /usr/bin/java, then export command will be* **export PATH=/usr/bin/java/bin:$PATH**

-   Run the following command to set **JAVA_HOME** environment variable.

        export JAVA_HOME=[PathToJDK]

-   You can verify that the environment variables necessary to run java jdk have been set correctly by running

        java --version

### 3.1.2  Install Maven and set up environment variables

-   Download Maven from here:

        http://ftp.mirror.tw/pub/apache/maven/maven-3/3.5.4/source/apache-maven-3.5.4-src.zip`

-   Unzip apache-maven-3.5.4-src.zip to folder:

        unzip apache-maven-3.5.4-src.zip

-   Download the SDK to the board by issuing the following command in PuTTY:

        git clone https://github.com/Azure/azure-iot-sdk-java.git

-   Update the PATH environment variable to include the full path to the bin folder containing Java. To ensure the correct path of Java run below command:     
       
        which mvn
        
3.  Ensure that the directory shown by the `which mvn` command matches one of the directories shown in your $PATH variable. You can confirm this by running following command.

        echo $PATH

4.  If maven path is missing in PATH environment variable, run following command to set the same.     

        export PATH=[PathToMvn]/bin:$PATH       

    ***Note***: *Here [PathToMvn] is output of `which mvn`. For example if `which mvn` output is /usr/bin/mvn, export command will be* **export PATH=/usr/bin/mvn/bin:$PATH**

-   You can verify that the environment variables necessary to run Maven 3 have been set correctly by running

        mvn --version

### 3.1.3  Build the Azure IoT Device SDK for Java


-   Run the following commands on device in sequence to build Azure IoT SDK.

        cd azure-iot-sdk-java/device
        mvn install | tee JavaSDK_Build_Logs.txt

-   Above command will generate the compiled JAR files with all dependencies. This bundle can be found at:

        azure-iot-sdk-java/device/iothub-java-client/target/iothub-java-client-{version}-with-deps.jar

## 3.2 Load the Azure IoT bits and prerequisites on device
### 3.2.1 Send Device Events to IoT Hub:

-   Navigate to the folder containing the executable JAR file for send event sample.

        cd azure-iot-sdk-java/device/samples/send-event/target

-   Run the sample by issuing following command.

      **If using AMQPS protocol:**

        java -jar ./send-event-{version}-with-deps.jar "{connection string}" "{number of requests to send}" "amqps"
    
    **If using HTTPS protocol:**

        java -jar ./send-event-{version}-with-deps.jar "{connection string}" "{number of requests to send}" "https"

    **If using MQTT protocol:**

        java -jar ./send-event-{version}-with-deps.jar "{connection string}" "{number of requests to send}" "mqtt"
          
    Replace the following in above command:
    
    -   `{version}`: Version of binaries you have build
    -   `{connection string}`: Your device connection string
    -   `{number of requests to send}`: Number of messages you want to send to IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

### 3.2.2 Receive messages from IoT Hub

-   Navigate to the folder containing the executable JAR file for the receive message sample.

        cd azure-iot-sdk-java/device/samples/handle-messages/target
     
-   Run the sample by issuing following command.

    **If using AMQPS protocol:**
   
        java -jar ./handle-messages-{version}-with-deps.jar "{connection string}" "amqps"
    
    **If using HTTPS protocol:**
   
        java -jar ./handle-messages-{version}-with-deps.jar "{connection string}" "https"

    **If using MQTT protocol:**
   
        java -jar ./handle-messages-{version}-with-deps.jar "{connection string}" "mqtt"
        
    Replace the following in above command:
    
    -   `{version}`: Version of binaries you have build
    -   `{connection string}`: Your device connection string
    -   `{number of requests to send}`: Number of messages you want to send to IoT Hub
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