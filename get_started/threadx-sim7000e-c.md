---
platform: threadx
device: sim7000e
language: c
---

Run a simple C sample on SIM7000E device running DAM
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

This document describes how to connect SIM7000E device running DAM with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK  and Run app on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   SIM7000E device.
-   DAM AZURE SDK and Snapdragon-llvm-4.0.11 tools and SIM7000 AT Upgrade Tool V1.2

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   [Please Contact us](http://www.simcomm2m.com/)

<a name="Build"></a>
# Step 3: Build and Run the sample

## 3.1 Load the Azure IoT bits

    
-   Download the SDK to the board by issuing the following command:

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Verify that you now have a copy of the source code under the `directory DAM\ AZURE\ SDK/azure-iot-sdk-c`.

<a name="Step-3-2-Build"></a>
## 3.2 Build the samples

There are one samples one for sending messages to IoT Hub and another for receiving messages from IoT Hub. Both samples only supports mqtt protocol.Follow the below instructions to edit the samples before building: 
    
### 3.2.1 Send Telemetry to IoT Hub Sample:

1.  Open the telemetry sample file in a text editor

        nano DAM\ AZURE\ SDK/test-app/src/azure_demo.c     

2.  Find the following placeholder for IoT connection string:

        static const char* connectionString = "[device connection string]";

3.  Replace the above placeholder with device connection string.
    
4.  Find the following place holder for editing protocol:

          // Select the MQTT Protocol to use with the connection
                #define USE_MQTT
        #ifdef USE_AMQP
            //protocol = AMQP_Protocol_over_WebSocketsTls;
            protocol = AMQP_Protocol;
        #endif
        #ifdef USE_MQTT
             protocol = MQTT_Protocol;
            //protocol = MQTT_WebSocket_Protocol;
        #endif
        #ifdef USE_HTTP
            //protocol = HTTP_Protocol;
        #endif
	
5.  So far we only support mqtt protocols. 

6.  Save your changes by pressing Ctrl+O and when nano prompts you to save it as the same file, just press ENTER.

7.  Press Ctrl+X to exit nano.

### 3.2.1 Send message from IoT Hub to Device Sample:

1.  Open the telemetry sample file in a text editor

        nano DAM\ AZURE\ SDK/test-app/src/azure_demo.c

2.  Follow same steps 1-7 as above to edit this sample.

### 3.2.1 Build the samples:

-   Build the SDK using following command. If you are facing any issues during build.

       cd AZURE SDK/build&&./build_azure_sdk_app_llvm.sh | tee LogFile.txt
    
    ***Note:*** *LogFile.txt in above command should be replaced with a file name where build output will be written.*
    
    *build\_azure\_sdk\_app\_llvm.sh create cust_app.bin under "AZURE SDK/build/bin".*

<a name="Step-3-3-Run"></a>
## 3.3 Run and Validate the Samples

In this section you will run the Azure IoT client SDK samples to validate
communication between your device and Azure IoT Hub. You will send messages to the Azure IoT Hub service and validate that IoT Hub has successfully receive the data. You will also monitor any messages send from the Azure IoT Hub to client.

### 3.3.1 Upload and Run App to SIM7000E:

1.  Use SIM7000 AT Upgrade Tool V1.2 upload the cust_app.bin to sim7000E custapp directory

2.  Restart sim7000E device 

3.  Use serial tools to connect sim7000 audio port witch 115200 Baud rate

4.  Switch to the azure tab of the app in serial tools

    **Command List:**

        Commands:
                       1. Help
                       2. Exit
                  Subgroups:
                     3. azure
                  > 3

5.  Enter "azure i <apn>" to get ip address 

6.  Enter "azure h" to start dns client 

7.  Enter "azure b" to create azure handle
 
### 3.3.2 Send Device Events to IOT Hub:

-   Run the sample by issuing following command.    

        Enter "azure s" to send event 

### 3.3.3 Receive messages from IoT Hub

-   Run the sample by issuing following command.

        Enter "azure g" to process received messages

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
