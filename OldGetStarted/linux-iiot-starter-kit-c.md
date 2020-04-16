---
platform: linux
device: iiot starter kit
language: c

---

Run a simple C sample on IIoT Starter Kit device running Linux
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [IIoT Starter Kit - Industry 4.0](#StarterKit)
-   [Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect IIoT Starter Kit device running Linux with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IIoT device
-   Build and deploy Azure IoT SDK on device

<a name="StarterKit"></a>
# IIoT Starter Kit - Industry 4.0

-   [TQ-Systems MBox-V][setup-devbox-linux]
-   [open62541 - open source stack][lnk-open62541]
-   [TSN Ready solution][lnk-kalycito]
-   Docker-ce

<a name="Build"></a>
# Build and Run the sample

## 1. Load the Azure IoT SDK and prerequisites on device

-   Open a PuTTY session and connect to the device.

-   Install the prerequisite packages by issuing the following commands from the command line on the device. Choose your commands based on the OS running on your device.

    **Debian or Ubuntu**

        sudo apt-get update
        sudo apt-get install -y curl uuid-dev libcurl4-openssl-dev build-essential cmake git

    **Fedora**

        sudo dnf check-update -y
        sudo dnf install uuid-devel libcurl-devel openssl-devel gcc-c++ make cmake git

    **Any Other Linux OS**

        Use equivalent commands on the target OS

    ***Note:*** *This setup process requires cmake version 2.8.12 or higher.* 
    
    *You can verify the current version installed in your environment using the  following command:*

        cmake --version

    *This library also requires gcc version 4.9 or higher. You can verify the current version installed in your environment using the following command:*
    
        gcc --version 

    *For information about how to upgrade your version of gcc on Ubuntu 14.04, see <http://askubuntu.com/questions/466651/how-do-i-use-the-latest-gcc-4-9-on-ubuntu-14-04>.*
    
-   Download the SDK to the board by issuing the following command in PuTTY:

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Verify that you now have a copy of the source code under the directory `~/azure-iot-sdk-c`.

<a name="Step-3-2-Build"></a>
## 2. Build the samples
    
### 2.1 Send Telemetry to IoT Hub Sample:

1.  Open the telemetry sample file in a text editor

        nano azure-iot-sdk-c/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample.c     

2.  Find the following placeholder for IoT connection string:

        static const char* connectionString = "[device connection string]";

3.  Replace the above placeholder with device connection string.
    
4.  Find the following place holder for editing protocol:

        #define SAMPLE_MQTT
        //#define SAMPLE_MQTT_OVER_WEBSOCKETS
        //#define SAMPLE_AMQP
        //#define SAMPLE_AMQP_OVER_WEBSOCKETS
        //#define SAMPLE_HTTP

5.  Please uncomment the protocol that you would like to run with and comment other protocols.

6.  Save your changes by pressing Ctrl+O and when nano prompts you to save it as the same file, just press ENTER.

7.  Press Ctrl+X to exit nano.

### 2.2 Send message from IoT Hub to Device Sample:

1.  Open the telemetry sample file in a text editor

        nano azure-iot-sdk-c/iothub_client/samples/iothub_ll_c2d_sample/iothub_ll_c2d_sample.c

2.  Follow same steps 1-7 as above to edit this sample.

### 2.3 Build the samples:

-   Build the SDK using following command. If you are facing any issues during build.

        sudo ./azure-iot-sdk-c/build_all/linux/build.sh | tee LogFile.txt
    
    ***Note:*** *LogFile.txt in above command should be replaced with a file name where build output will be written.*
    
    `build.sh` creates a folder called **cmake** under **~/azure-iot-sdk-c/**. Inside "cmake" are all the results of the compilation of the complete software.

<a name="Step-3-3-Run"></a>
## 3. Run and Validate the Samples

### 3.1 Send Device Events to IOT Hub:

-   Run the sample by issuing following command.    

        sudo azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample

-   Use the [Device Explorer tool](https://github.com/Azure/azure-iot-sdk-csharp/releases "Device Explorer tool") to view the device data
-   [Refer the DeviceExplorer usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md "Refer the DeviceExplorer usage document")

### 3.2 Receive messages from IoT Hub

-   Run the sample by issuing following command.

        sudo azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_ll_c2d_sample/iothub_ll_c2d_sample

-   Use the [Device Explorer tool](https://github.com/Azure/azure-iot-sdk-csharp/releases "Device Explorer tool") to view the device data

-   [Refer the DeviceExplorer usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md "Refer the DeviceExplorer usage document")

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Quickstart : Deploy IoT Edge module to a Linux device]
-   [Save IoT Hub messages to Azure data storage]
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
-   [Remote monitoring and notifications with Logic Apps]   
-   [Use as Embedded PC to connect in a TSN network]
-   Send OPCUA pubsub (Part 14) data to Azure cloud

[Quickstart : Deploy IoT Edge module to a Linux device]: https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux
[Save IoT Hub messages to Azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[Remote monitoring and notifications with Logic Apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[Use as Embedded PC to connect in a TSN network]: https://www.kalycito.com/articles/machine-to-machine-communication-using-opc-ua-tsn/

For more details contact:
<http://www.kalycito.com/>

[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-open62541]: https://open62541.org/
[lnk-kalycito]: http://www.kalycito.com/
