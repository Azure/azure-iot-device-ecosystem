---
platform: embedded linux
device: anylink da
language: c
---

Run a simple C sample on AnyLink DA device running Embedded Linux
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Tips](#tips)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect AnyLink DA device running linux with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   AnyLink DA device
-   Cross compiler toolchain

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

Please use a SSH client to connect AnyLink DA device.

<a name="Build"></a>
# Step 3: Build and Run the sample

***Note:*** *In this section, you should build the SDK and sample on the host PC(Ubuntu14.04), not on the device.*

## 3.1 Build SDK with cross compile toolchains 

### 3.1.1 Setting up the building environment

-   Install the prerequisite packages by issuing the following commands from the command line on the PC. Choose your commands based on the OS running on your PC.

        sudo apt-get update

        sudo apt-get install -y cmake git

    ***Note:*** *This setup process requires cmake version 2.8.12 or higher.* 
    
    *You can verify the current version installed in your environment using the  following command:*

        cmake --version

-   Get cross compile toolchain from [here][cross-compile-toolchain]
    
-   Download the SDK to the board by issuing the following command:

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Verify that you now have a copy of the source code under the directory ~/azure-iot-sdk-c.

### 3.1.2 Setting up cmake to cross compile

- See [Cross Compiling the Azure IoT Hub SDK][lnk-azure-iot-cross-compile] to learn how to cross compiling the Azure IoT Hub SDK. Here some important information.

- In order to tell cmake that it is performing a cross compile we need to provide it with a toolchain file. To save us some typing and to make our whole procedure less dependent upon the current user we are going to export a value. Whilst in the directory of the cross compile toolchain enter the following command

		cd [the installation path of cross compile toolchain]/4.5.1/arm-none-linux-gnueabi
		export RPI_ROOT=$(pwd)

- Edit/Create the following file using any text editor of your choice (this file is necessary to cross build the SDK):

		azure-iot-sdk-c/build_all/linux/toolchain-arm.cmake

	- Insert the following lines to the file:

			INCLUDE(CMakeForceCompiler)

			SET(CMAKE_SYSTEM_NAME Linux)     # this one is important
			#SET(CMAKE_SYSTEM_VERSION 1)     # this one not so much

			# this is the location of the toolchain 
			SET(CMAKE_C_COMPILER $ENV{RPI_ROOT}/../bin/arm-linux-gcc) 

			# this is the file system root of the target
			SET(CMAKE_FIND_ROOT_PATH $ENV{RPI_ROOT}/sys-root/)

			SET(OPENSSL_ROOT_DIR $ENV{RPI_ROOT}/sys-root/usr/lib)
			SET(OPENSSL_INCLUDE_DIR $ENV{RPI_ROOT}/sys-root/usr/include)

			SET(CURL_ROOT_DIR $ENV{RPI_ROOT}/sys-root/usr/lib)
			SET(CURL_INCLUDE_DIR $ENV{RPI_ROOT}/sys-root/usr/include)

			SET(UUID_ROOT_DIR $ENV{RPI_ROOT}/sys-root/usr/lib)
			SET(UUID_INCLUDE_DIR $ENV{RPI_ROOT}/sys-root/usr/include)

			# search for programs in the build host directories
			SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)

			# for libraries and headers in the target directories
			SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
			SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
	
- See [Build the samples](#Step-3-2-Build) to learn how to build the samples.


<a name="Step-3-2-Build"></a>
## 3.2 Build the samples

There are two samples one for sending messages to IoT Hub and another for receiving messages from IoT Hub. Both samples supports different protocols. You can make modification to the samples with your choice of protocol before building the samples. By default the samples will build for AMQP protocol.  Follow the below instructions to edit the samples before building: 
    
### 3.2.1 Send Telemetry to IoT Hub Sample:

1.  Open the telemetry sample file in a text editor

		nano azure-iot-sdk-c/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample.c     

2. Find the following placeholder for IoT connection string:

        static const char* connectionString = "[device connection string]";

3. Replace the above placeholder with device connection string.
    
4. Find the following place holder for editing protocol:

          // Select the Protocol to use with the connection
		#ifdef USE_AMQP
		    //protocol = AMQP_Protocol_over_WebSocketsTls;
		    protocol = AMQP_Protocol;
		#endif
		#ifdef USE_MQTT
		    //protocol = MQTT_Protocol;
		    //protocol = MQTT_WebSocket_Protocol;
		#endif
		#ifdef USE_HTTP
		    //protocol = HTTP_Protocol;
		#endif
	
5. Please uncomment the protocol that you would like to test with and comment other protocols. If testing for multiple protocols, please repeat above step for each protocol. 

6. Save your changes by pressing Ctrl+O and when nano prompts you to save it as the same file, just press ENTER.

7. Press Ctrl+X to exit nano.

### 3.2.1 Send message from IoT Hub to Device Sample:

1. Open the telemetry sample file in a text editor

	 	nano azure-iot-sdk-c/iothub_client/samples/iothub_ll_c2d_sample/iothub_ll_c2d_sample.c

2. Follow same steps 1-7 as above to edit this sample.

### 3.2.2 Build the samples:

-   Build the SDK using following command. If you are facing any issues during build.

    	./azure-iot-sdk-c/build_all/linux/build.sh --toolchain-file ./azure-iot-sdk-c/build_all/linux/toolchain-rpi.cmake -cl --sysroot=$RPI_ROOT/sys-root | tee LogFile.txt

    
    ***Note:*** *LogFile.txt in above command should be replaced with a file name where build output will be written.*
    
    *build.sh creates a folder called "cmake" under "~/azure-iot-sdk-c/". Inside "cmake" are all the results of the compilation of the complete software.*


<a name="Step-3-3-Run"></a>
## 3.3 Run and Validate the Samples

In this section you will run the Azure IoT client SDK samples to validate
communication between your device and Azure IoT Hub. You will send messages to the Azure IoT Hub service and validate that IoT Hub has successfully receive the data. You will also monitor any messages send from the Azure IoT Hub to client.

### 3.3.1 Copy the built image to the device

-   Copy following files to AnyLink DA:

    libraries:

        libaziotsharedutil.a
        libiothub_client_mqtt_ws_transport.a 
        libiothub_client.a                    
        libiothub_service_client.a
        libiothub_client_amqp_transport.a     
        libserializer.a
        libiothub_client_amqp_ws_transport.a  
        libuamqp.a
        libiothub_client_http_transport.a     
        libuhttp.a
        libiothub_client_mqtt_transport.a     
        libumqtt.a

    samples:

        iothub_ll_telemetry_sample


### 3.3.2 Send Device Events to IOT Hub:

-   Run the sample by issuing following command.    

		azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample


### 3.3.3 Receive messages from IoT Hub

-   Run the sample by issuing following command.

		azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_ll_c2d_sample/iothub_ll_c2d_sample
		

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
[lnk-azure-iot-cross-compile]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/iotcertification/templates/template-linux-c.md
[cross-compile-toolchain]: http://www.anylink.io/d/arm-linux-gcc-4.5.1.tar.gz
