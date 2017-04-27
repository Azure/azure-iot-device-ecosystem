---
platform: debian
device: armadillo iot gateway g3
language: java
---

Run a simple JAVA sample on Armadillo-IoT Gateway G3 device running Debian jessie
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"/></a>
# Introduction

**About this document**

This document describes how to connect Armadillo-IoT Gateway G3 device running Debian jessie with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment](#Prepare_dev_env)
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Armadillo-IoT Gateway G3 device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-   Follow the instruction described this [page](http://manual.atmark-techno.com/armadillo-iot/armadillo-iotg-std_startup_guide_ja-2.3.4/) for setup the device.

-   Install packages which are necessary to running the IoT SDK samples:

    -   Make the default user to sudoer

        ```
        $ su
        # apt-get update
        # usermod -G sudo atmark
        # exit
        $ exit
        ```

    -   Install other packages via sudo

            sudo apt-get install -y uuid openssh-server

    -   Create the home directory of the default user if it not exists:

        ```
        sudo mkdir /home/atmark
        sudo chown /home/atmark
        sudo chgrp /home/atmark
        ```

    -   For OpenJDK8 and maven, modify the sources.list

        ```
        sudo cp /etc/apt/sources.list /etc/apt/sources.list.jessie
        sudo sh -c "echo 'deb http://ftp.jp.debian.org/debian jessie-backports main contrib non-free' >> /etc/apt/sources.list"
        sudo apt-get update
        ```

    -   Install packages via sudo

            sudo apt-get install -y openjdk-8-jdk maven


-   Run the following command to set **JAVA_HOME** environment variable.

        export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-armhf

    -   Make sure that the JAVA_HOME environment variable includes the full path to the JDK.

            update-alternatives --config java

-   Recover the sources.list

        sudo cp /etc/apt/sources.list.jessie  /etc/apt/sources.list
        sudo apt-get update

-   Prepare the directory for [the next step](#Step_3_1):

        mkdir -p ~/work/IoT_SDK_Java

<a name="Build"></a>
# Step 3: Build SDK and Run the sample

<a name="Step_3_1"/></a>
## 3.1 Build SDK and sample with ATDE

***In this section, you build the SDK and sample with the ATDE on the host PC, not on the device*** (You can build them on the device, but it may need too long time.)

-   Install the prerequisite packages for the Microsoft Azure IoT Device SDK for Java by issuing the commands from the command line on the ATDE which given [here](#Prepare_dev_env).

-   Run following commands:

        mkdir -p ~/work/IoT_SDK_Java
        cd ~/work/IoT_SDK_Java

-   Download the Microsoft Azure IoT Device SDK for Java to the board by issuing the following command on the ATDE:

        git clone https://github.com/Azure/azure-iot-sdk-java.git

-   Build the SDK using following command. 

        cd azure-iot-sdk-java/device
        mvn install -DskipTests=true | tee JavaSDK_Build_Logs.txt

### 3.1.1 Copy the built image to the device:
-   Archive the two directories (azure-iot-sdk-java/ and ~/.m2/repository/):

    ```
    cd ~/work/IoT_SDK_Java
    tar cvf azure-iot-sdk-java.tar ./azure-iot-sdk-java
    tar cvf repository.tar -C ~/.m2 ./repository
    ```

-   Copy above two archives to device via network:

        scp azure-iot-sdk-java.tar atmark@{device's IP address}:~/work/IoT_SDK_Java/
        scp repository.tar atmark@{device's IP address}:~/work/IoT_SDK_Java/


-   On device, extract the archive and run the maven to execute SDK's test cases.

    ```
    cd ~/work/IoT_SDK_Java
    tar xvf ./azure-iot-sdk-java.tar
    mkdir ~/.m2
    tar xvf repository.tar -C ~/.m2
    cd azure-iot-sdk-java/device
    mvn install | tee JavaSDK_Build_Logs.txt
    ```

<a name="Step_3_2"/></a>
## 3.2 Run and Validate the Samples

<a name="Step_3_2_1"/></a>
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

<a name="Step_3_2_2"/></a>
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
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md


<a name="Prepare_dev_env"></a>
## Prepare your development environment

-   Download the ATDE v6 (Atmark Techno Development Environment v6) from here: [http://armadillo.atmark-techno.com/atde](http://armadillo.atmark-techno.com/atde)

-   Open the .vmx image file in the downloaded archive with VMware.

-   When after ATDE is booted, issue following commands in console (for install OpenJDK8 and maven):

    ```
    sudo apt-get install -y uuid uuid-dev
    sudo cp /etc/apt/sources.list /etc/apt/sources.list.jessie
    sudo sh -c "echo 'deb http://ftp.jp.debian.org/debian jessie-backports main contrib non-free' >> /etc/apt/sources.list"
    sudo apt-get update
    sudo apt-get install -y openjdk-8-jdk maven
    ```

-   Run the following command to set **JAVA_HOME** environment variable.

        export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-i386

-   Make sure that the JAVA_HOME environment variable includes the full path to the JDK.

        update-alternatives --config java


