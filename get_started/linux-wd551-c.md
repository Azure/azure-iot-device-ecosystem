---
platform: linux
device: wd551
language: c
---

Run a simple C sample on WD551 device running Linux
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

This document describes how to connect WD551 device running Linux with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Prepare a ubuntu14.04 environment and install the arm-none-linux-gnueabi 2014.05 cross-compile tool
-   Download libuuid-1.0.3/curl-7.69.1/openssl-1.1.1g for linux code source

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   On the Host PC, you should use the PuTTY to connect to the device, and configuring the serial port at a baud rate of 115200.
-   Turn on the debugging switch of the device.
-   Connect the device to internet.

<a name="Build"></a>
# Step 3: Build and Run the sample

## 3.1 Load the Azure IoT bits and prerequisites on device

-   Install the prerequisite packages by issuing the following commands from the command line on the host pc. 

        sudo apt-get update
        sudo apt-get install -y cmake git

-   Configure GCC as arm-none-Linux-gnueabi-gcc on host pc.
-   Cross-compile the dependent libraries (libuuid curl openssl) needed on the device on the host PC.
-   Download the SDK to the Host PC by issuing the following command:

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Verify that you now have a copy of the source code under the directory ~/azure-iot-sdk-c on the host pc.

## 3.2 Build the samples

Follow the below instructions to edit the samples before building: 

### 3.2.1 Convenience Sample:

1.  Open the telemetry sample file in a text editor

		nano azure-iot-sdk-c/iothub_client/samples/iothub_convenience_sample/iothub_convenience_sample.c   

2.  Find the following placeholder for IoT connection string:

        static const char* connectionString = "[device connection string]";

3.  Replace the above placeholder with device connection string.
    
4.  Find the following place holder for editing protocol:

		//Enable the SAMPLE_MQTT Protocol, and disable other Protocol.

		// Select the Protocol to use with the connection
		#ifdef SAMPLE_MQTT
			protocol = MQTT_Protocol;
		#endif // SAMPLE_MQTT
		#ifdef SAMPLE_MQTT_OVER_WEBSOCKETS
			//protocol = MQTT_WebSocket_Protocol;
		#endif // SAMPLE_MQTT_OVER_WEBSOCKETS
		#ifdef SAMPLE_AMQP
			//protocol = AMQP_Protocol;
		#endif // SAMPLE_AMQP
		#ifdef SAMPLE_AMQP_OVER_WEBSOCKETS
			//protocol = AMQP_Protocol_over_WebSocketsTls;
		#endif // SAMPLE_AMQP_OVER_WEBSOCKETS
		#ifdef SAMPLE_HTTP
			//protocol = HTTP_Protocol;
		#endif // SAMPLE_HTTP

5.  Modify callback function.

		receive_msg_callback
		device_method_callback
		connection_status_callback
		send_confirm_callback

6.  Save your changes by pressing Ctrl+O and when nano prompts you to save it as the same file, just press ENTER.

7.  Press Ctrl+X to exit nano.

### 3.2.2 Build the samples:

-   Modify the CMakeLists.txt file int the azure-iot-sdk-c, and make sure you can link to your compiled dependent library.
-   Build the SDK using following command. If you are facing any issues during build.

        sudo ./azure-iot-sdk-c/build_all/linux/build.sh | tee LogFile.txt
    
    ***Note:*** *LogFile.txt in above command should be replaced with a file name where build output will be written.*
    
    *build.sh creates a folder called "cmake" under "~/azure-iot-sdk-c/". Inside "cmake" are all the results of the compilation of the complete software.*

## 3.3 Run and Validate the Samples

In this section you will run the Azure IoT client SDK samples to validate
communication between your device and Azure IoT Hub. You will send messages to the Azure IoT Hub service and validate that IoT Hub has successfully receive the data. You will also monitor any messages send from the Azure IoT Hub to client.

### 3.3.1 Send Device Events to IOT Hub,Receive messages from IoT Hub,Receive Method form IoT Hub:

-   Copy the iothub_convenience_sample file to Udisk from host pc.

		//Execute the shell command on host pc.
		cp azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_convenience_sample/iothub_convenience_sample /usb/sda1/

-   Copy the iothub_convenience_sample to device from Udisk.

		//Use PuTTY connect the device on host pc, and execute the shell command.
		cp /usb/sda1/iothub_convenience_sample /tmp/

-   Run the iothub_convenience_sample on device.

		//Use PuTTY connect the device on host pc, and execute the shell command. 
		chmod 777 /tmp/iothub_convenience_sample
		/tmp/iothub_convenience_sample

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