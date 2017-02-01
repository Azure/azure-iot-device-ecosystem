---
platform: windows 10 iot enterprise
device: xk-flx
language: java
---

Run a simple JAVA sample on XK-FLX device running Windows 10 IoT Enterprise
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

This document describes how to connect XK-FLX device running Windows 10 IoT Enterprise with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-windows]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   XK-FLX device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
In this section, you will register your device using DeviceExplorer. The DeviceExplorer is a Windows application that interfaces with Azure IoT Hub and can perform the following operations:

-   Device management
    -   Create new devices
    -   List existing devices and expose device properties stored on Device Hub
    -   Provides ability to update device keys
    -   Provides ability to delete a device
-   Monitoring events from your device
-   Sending messages to your device

To run DeviceExplorer tool, use your IoT Hub connection string generated from the Azure web portal

-   IoT Hub Connection String

**Steps:**

1.  Click [here](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/readme.md) to download and install DeviceExplorer.

2.  Add connection information under the **Configuration** tab and click the **Update** button.

3.  Create and register the device with your IoT Hub using instructions as below.

    a. Click the **Management** tab.    
    
    b. Your registered devices will be visible in the list. In case your device is not there in the list, click **Refresh** button. If this is your first time, then you shouldn't retrieve anything.
       
    c. Click **Create** button to create a device ID and key. 
    
    d. Once created successfully, device will be listed in DeviceExplorer. 
    
    e. Right click the device and from context menu select "**Copy connection string for selected device**".
    
    f. Save this information in Notepad. You will need this information in later steps.


<a name="Build"></a>
# Step 3: Build SDK and Run the sample

<a name="Step_3_1"></a>
## 3.1 Install Azure IoT Device SDK and prerequisites on device

-   To run the SDK you will need Java SE 1.8.

-   Install the prerequisite packages using command line on the device.

<a name="Step_3_1_1"></a>
### 3.1.1  Install Java JDK 1.8 and set up environment variables
        
1.  For downloads and installation instructions go here: <http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>
       
2.  Please make sure that the `PATH` environment variable includes the full path to the jdk1.8.x\bin directory. (Example: c:\Program Files\Java\jdk1.8.0_65)
        
3.  Please make sure that the `JAVA_HOME` environment variable includes the full path to the jdk1.8.x directory. (Example: JAVA_HOME=c:\Program Files\Java\jdk1.8.0_65)

4.  You can test whether your PATH variable is set correctly by restarting your console and running `java -version`.

<a name="Step_3_1_2"></a>
### 3.1.2  Install Maven and set up environment variables
Using Maven 3 is the recommended way to install Azure IoT device SDK for Java.

1.  For downloads and installation instructions for maven 3 go here: <https://maven.apache.org/download.cgi>

2.  Please make sure that the PATH environment variable includes the full path to the apache-maven-3.x.x\bin directory. (Example: F:\Setups\apache-maven-3.3.3\bin). The apache-maven-3.x.x directory is where Maven 3 is installed.

2.  You can verify that the environment variables necessary to run Maven 3 have been set correctly by restarting your console and running `mvn --version.`
  
<a name="Step_3_1_3"></a>
### 3.1.3  Install GIT

-   For downloads and installation instructions go here:
<http://git-scm.com/book/en/v2/Getting-Started-Installing-Git>


<a name="Step_3_1_4"></a>
### 3.1.4 Build the Azure IoT Device SDK for Java

1.  Download the SDK to the board by issuing the following command:

        git clone https://github.com/Azure/azure-iot-sdks.git

2.  Verify that you now have a copy of the source code under the directory **azure-iot-sdks**.

3.  Run the following commands on device in sequence to build Azure IoT SDK.

        cd azure-iot-sdk-java/device
        mvn install

4.  Above command will generate the compiled JAR files with all dependencies. This bundle can be found at:

        azure-iot-sdk-java/device/iothub-java-client/target/iothub-java-client-{version}-with-deps.jar

<a name="Step_3_2"></a>
## 3.2 Run and Validate the Samples

<a name="Step_3_2_1"></a>
### 3.2.1 Send Device Events to IoT Hub:

-   Navigate to the folder containing the executable JAR file for send event sample.

        cd /azure-iot-sdk-java/device/samples/send-event/target

-   Run the sample by issuing following command.

    **If using HTTPS protocol:**

        java -jar ./send-event-{version}-with-deps.jar "{connection string}" "{number of requests to send}" "https"
		
    Replace the following in above command:
    
    -   `{version}`: Version of binaries you have build
    -   `{connection string}`: Your device connection string
    -   `{number of requests to send}`: Number of messages you want to send to IoT Hub

-   On Windows, refer "Monitor device-to-cloud events" in [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to see the data your device is sending.

<a name="Step_3_2_2"></a>
### 3.2.2 Receive messages from IoT Hub

-   Navigate to the folder containing the executable JAR file for the receive message sample.

        cd /azure-iot-sdk-java/device/samples/handle-messages/target
     
-   Run the sample by issuing following command.
    
    **If using HTTPS protocol:**
   
        java -jar ./handle-messages-{version}-with-deps.jar "{connection string}" "https"
        
    Replace the following in above command:
    
    -   `{version}`: Version of binaries you have build
    -   `{connection string}`: Your device connection string
    -   `{number of requests to send}`: Number of messages you want to send to IoT Hub

-   On Windows, refer "Send cloud-to-device messages" in [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) for instructions on sending messages to device.

[setup-devbox-windows]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/get_started/java-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md