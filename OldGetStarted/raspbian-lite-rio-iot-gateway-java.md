---
platform: raspbian lite
device: rio iot gateway
language: java
---

Run a simple JAVA sample on RIO IoT Gateway device running Raspbian Lite
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

This document describes how to connect RIO IoT Gateway device running Raspbian Lite with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   RIO IoT Gateway device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-   Install the latest Raspbian operating system on your RIO IoT Gateway.
-   After installation, the Raspberry Pi configuration menu is ready. You can set the date/time for your region. Under Advanced Options, enable ssh so you can access the device remotely with Putty.
-   Connect RIO device to your network using an ethernet cable or a wireless
-   You need the configure IP address of RIO device in order to connect over the network.
-   Then, you will be able to see that your device is working, 
-   Open SSH terminal program (Putty)
-   Enter IP address and Port (22) and select Connection Type as SSH 
-   When connection is done, login with username (pi) and password (raspberry)

<a name="Build"></a>
# Step 3: Build SDK and Run the sample
If you have successfully prepared your development environment the IoT device SDK and all samples should be built and ready to use.

-   Start a new instance of the Device Explorer, select or create a new device, obtain and note the connection string for the device, and begin monitoring under the Data tab.
-   Build the client library alonf with the samples.

		{IoT device SDK root}/java/device/>mvn install -DskipTests
		
    On build completion, you will see summary of what got built. 
   
-   Navigate to the folder containing the executable JAR file for the sample that you wish to run and run the samples as follows:

    For example, the executable JAR file for sending and receving event, send-receive-sample can be found at:

        {IoT device SDK root}/java/device/samples/send-receive-sample/target/send-event-{version}-with-deps.jar
    
	Navigate to the directory with the sample. Run the sample using the following command:        

        java -jar ./send-receive-sample-{version}-with-deps.jar "{connection string}" "{number of requests to send}" "{https or amqps or mqtt or amqps_ws }"
 
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

