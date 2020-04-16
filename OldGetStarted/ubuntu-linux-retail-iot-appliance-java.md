---
platform: ubuntu linux
device: retail iot appliance
language: java
---

Run a simple JAVA sample on Retail IoT Appliance device running Ubuntu Linux
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

This document describes how to connect Retail IoT Appliance device running Ubuntu Linux with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Core Appliance device.
-   Console Cable USB to RJ45, Putty

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Connect the RJ45 endpoint of the console cable to Retail IoT Appliance and USB to a PC
-   Connect the Core Appliance with ethernet cable to network.
-   Power the Retail IoT Appliance On
-   Wait about 30 seconds for the Retail IoT Appliance starting.

<a name="Build"></a>
# Step 3: Build SDK and Run the sample

<a name="Step_3_1"></a>
## 3.1 Install Azure IoT Device SDK and prerequisites on device

-   Open a PuTTY session and connect to the device.
    -   Use Device Manager of Windows to find which COM is created for the Console Cable
    -   Use Putty to open the COM you found with “Speed” 115200
    -   Type ENTER in Putty, you will see you login the Retail IoT Appliance.
    -   Input “exit” and hit Enter to exit a service tool of Core Appliane and back to user’s bash.
-   Install the prerequisite packages by issuing the following commands from the command line on the device.

<a name="Step_3_1_1"></a>
### 3.1.1  Install Java JDK and set up environment variables
        
1.  Install command Ubuntu 14.04

        sudo apt-get update
        sudo apt-get install openjdk-8-jdk

2.  Check java

        /usr/bin/java

3.  Check PATH

        echo $PATH
        /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games

4.  Check Java path is in PATH environment variable.

    if you run “update-alternatives --config java”, you will see:

        $update-alternatives --config java

    There are 2 choices for the alternative java (providing /usr/bin/java).

    | Selection | Path | Priority | Status |
    |-----------|------| ---------|--------|
    | 0 | /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java| 1069 | auto mode |
    | 1 | /opt/Oracle_Java/jre1.8.0_60/bin/java | 1 | manual mode |
    | 2 | /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java | 1069 | manual mode |

    Press enter to keep the current choice[*], or type selection number:

5.  Check Java JRE is already in the Retail IoT Appliance.

        $java -version
        java version "1.8.0_60"
        Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
        Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)


6.  Set JAVA_HOME environment variable

        export JAVA_HOME= /usr/lib/jvm/java-8-openjdk-amd64/

<a name="Step_3_1_2"></a>
### 3.1.2  Install Maven and set up environment variables

1.  Install maven

        sudo apt-get install maven

2.  Check which mvn

        /usr/bin/mvn

3.  Check PATH

        echo $PATH
        /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games

4.  Check maven

        $mvn –version
        Apache Maven 3.0.5
        Maven home: /usr/share/maven
        Java version: 1.8.0_171, vendor: Oracle Corporation
        Java home: /usr/lib/jvm/java-8-openjdk-amd64/jre
        Default locale: en_US, platform encoding: UTF-8
        OS name: "linux", version: "3.13.0-32-generic", arch: "amd64", family: "unix"

<a name="Step_3_1_3"></a>
### 3.1.3  Install GIT

1.  Install git

        sudo apt-get install git

<a name="Step_3_1_4"></a>
### 3.1.4 Build the Azure IoT Device SDK for Java

1.  Download the SDK to the board by issuing the following command in PuTTY:

        git clone https://github.com/Azure/azure-iot-sdk-java.git

2.  Verify that you now have a copy of the source code under the directory `azure-iot-sdk-java` we can see it, it’s OK.

3.  Run the following commands on device in sequence to build Azure IoT SDK

        cd azure-iot-sdk-java/device
        mvn install | tee JavaSDK_Build_Logs.txt

4.  Above command will generate the compiled JAR files with all dependencies. This bundle can be found at:

         azure-iot-sdk-java/device/iot-device-client/target/iot-device-client-1.11.1.jar

<a name="Step_3_2"></a>
## 3.2 Run and Validate the Samples

<a name="Step_3_2_1"></a>
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

<a name="Step_3_2_2"></a>
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
[setup-devbox-linux]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/get_started/java-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
