---
platform: linux
device: four-faith r100
language: c
---

Run a simple C sample on Four-faith R100 device running Linux
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Step-1-Prerequisites)
-   [Step 2: Prepare your Device](#Step-2-PrepareDevice)
-   [Step 3: Build and Run the Sample](#Step-3-Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

The following document describes the process of connecting a Four-faith R100 device to Azure IoT Hub.This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Step-1-Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Computer with a Git client installed so that you can access the azure-iot-sdk-c code on GitHub.
-   A Four-faith R100 device.
-   **Ubuntu x86_32 machine** (for cross compiling ddwrt firmware) 

<a name="Step-2-PrepareDevice"></a>
# Step 2: Prepare your Device
-   Connect the Four-faith R100 using the Ethernet cable
-   Configure R100 like any other Router, make sure it can connect to network

<a name="Step-3-Build"></a>
# Step 3: Build and Run the sample

<a name="Setup"></a>
## Setup the development environment

This section shows you how to set up a development environment for the Azure IoT device SDK for C on Four-faith R100.

-   git clone a copy of azure-sdk-c code like:

	`git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git`
	
-   add strings:
	<pre><code>
	link_libraries("libdl.so")
	link_directories("/opt/4.9.2_0.9.33/extlib/uuid/lib")
	link_directories("/opt/4.9.2_0.9.33/extlib/libidn/lib")
	link_directories("/opt/4.9.2_0.9.33/extlib/zlib/lib")
	link_directories("/opt/4.9.2_0.9.33/extlib/curl/lib") 
	</code></pre>
	to azure-iot-sdk-c/CMakeLists.txt

-   modify azure-iot-sdk-c/c-utility/CMakeLists.txt, add:
	<pre><code>
	include_directories("/opt/4.9.2_0.9.33/extlib/uuid/include")
	include_directories("/opt/4.9.2_0.9.33/extlib/openssl/include")
	include_directories("/opt/4.9.2_0.9.33/extlib/curl/include")
	</code></pre>

-   enter azure-iot-sdk-c/build_all/linux, and create a 'toolchain-ddwrt.cmake' file:

	<pre><code>
	INCLUDE(CMakeForceCompiler)

	SET(CMAKE_SYSTEM_NAME Linux)     # this one is important
	SET(CMAKE_SYSTEM_VERSION 1)     # this one not so much
	
	#configure mipsel-openwrt-linux-gcc path
	SET(CMAKE_C_COMPILER /opt/4.9.2_0.9.33/bin/mipsel-linux-uclibc-gcc --std=c99)
	
	SET(OPENSSL_INCLUDE_DIR "/opt/4.9.2_0.9.33/extlib/openssl/include/openssl")
	SET(OPENSSL_ROOT_DIR "/opt/4.9.2_0.9.33/extlib/openssl/bin")
	SET(OPENSSL_CRYPTO_LIBRARY "/opt/4.9.2_0.9.33/extlib/openssl/lib/libcrypto.so")
	SET(OPENSSL_SSL_LIBRARY "/opt/4.9.2_0.9.33/extlib/openssl/lib/libssl.so")
	SET(CURL_LIBRARY "/opt/4.9.2_0.9.33/extlib/curl/lib/libcurl.so")
	SET(CURL_INCLUDE_DIR "/opt/4.9.2_0.9.33/extlib/curl/include/curl")
	
	
	# search for programs in the build host directories
	SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
	
	# for libraries and headers in the target directories
	#SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
	SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
	</code></pre>

-   modify the variables below in 'toolchain-ddwrt.cmake' according to your own system:

	<pre><code>
	  CMAKE_C_COMPILER 
	  OPENSSL_INCLUDE_DIR  
	  OPENSSL_ROOT_DIR   
	  OPENSSL_CRYPTO_LIBRARY  
	  OPENSSL_SSL_LIBRARY 
	</pre></code>

-   And then run `sudo chmod +x toolchain-ddwrt.cmake`
-   copy and uncompress '4.9.2_0.9.33_azure_32bit.tar.gz' to ubuntu like `/opt`

 <a name="build"></a>
## Build the sample

-   modify files below in a text editor (for example vi):
	<pre><code>
	 azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http.c
	 azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp.c
	 azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt.c
	 azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp_websockets/iothub_client_sample_amqp_websockets.c
	 azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_mqtt_websockets/iothub_client_sample_mqtt_ws.c
	</pre></code>  

-   Locate the following code in the file:
```
static const char* connectionString = "[device connection string]";
```
- Replace "[device connection string]" with the device connection string you noted [earlier](#beforebegin). Save the changes.
- Run `./build.sh --toolchain-file toolchain-ddwrt.cmake -cl` in the **azure-iot-sdk-c/build\_all/linux** directory.   

<a name="deploy"></a>
## Deploy the sample

-   Enter **azure-iot-sdk-c/cmake/iotsdk\_linux/iothub\_client/samples**   
The samples are all here, we can use samba to share this folder to win7  
and on win7 we can run a http server like hfs.exe and add the sample files to it to transfer

-   Transfer the sample executable files to R100  
	after adding the files to hfs.exe we can use wget on R100 device:
	<pre><code>
	cd /tmp/
	wget http://192.168.1.59/iothub_client_sample_http
	chmod +x iothub_client_sample_http
	</pre></code>


<a name="run"></a>
## Run the sample

-   Run the sample **/tmp/iothub\_client\_sample\_http**  
	before running these samples, we may launch DeviceExplorer.exe on win7 first  
	and click "Monitor" to Monitor Iot hub events,and then lanuch our sample on R100
	<pre><code>
	cd /tmp/
	./iothub_client_sample_http
	</pre></code>   
   If everything goes fine we may see some data output in DeviceExplorer "Event Hub Data"  
   **Note:**:  [How to use DeviceExplorer]

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
[setup-iothub]: ../setup_iothub.md
[lnk-manage-iothub]: ../manage_iot_hub.md
[How to use DeviceExplorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer



