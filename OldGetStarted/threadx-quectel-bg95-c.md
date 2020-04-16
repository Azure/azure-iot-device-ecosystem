---
platform: threadx
device: quectel bg95
language: c
---

Run a simple C sample on Quectel BG95 device running ThreadX
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

This document describes how to connect Quectel BG95 device running ThreadX with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Quectel BG95 device
-   Quectel BG95 SDK Package and Quectel_LTE&5G_Windows_USB_Driver
-   Download and install [Python](https://www.python.org/downloads/)
-   Provide Network connectivity (SIM Card) supported by the device
-   Cross compile tool LLVM and porting-layer tool

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
### Hardware Environmental setup
1.  Install Drivers in your PC, the installation file located in Quectel_LTE&5G_Windows_USB_Driver folder

2.  Connect Quectel BG95 modules to board and Insert a SIM card to provide network

3.  Connect main antenna and GNSS antenna to corresponding interface

4.  Connect RS232-to-USB cable and USB cable to PC

5.  Turn on **POWER** switch and press **PWRKEY** to start the device

### Software Environmental setup
1.  Install QPST tool and Serial Port Utility in PC

2.  Create new folder Quectel_BG95 and copy Quectel_BG95\_QuecOpen\_SDK\_Package\_V1.0.1, LLVM tool, porting-layer tool to the new folder

3.  Open device manager, check if COM Prot and serial port are identified by PC

4.  Open QPST Configuration -> Start Clients -> EFS Explore -> Select COM port -> Alternate File System. Navigate to dataX folder

5.  Open Serial Port Utility -> Select USB Serial Port -> set Baudrate to 115200 -> open USB Serial Port.

<a name="Build"></a>
# Step 3: Build and Run the sample

## 3.1 Build Dependent Libs

Navigate to Quectel_BG95 and download the SDK

	git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

Verify that you now have a copy of the source code under the directory `C:\Quectel_BG95\azure-iot-sdk-c`.


Open azure_porting_layer_src folder, there are `build_azure_sdk_lib_llvm.bat` and `build_azure_sdk_port_lib_llvm.bat` file. Modify the two files as the following configuration:

For `build_azure_sdk_lib_llvm.bat`:

	set LLVMROOT=C:\Quectel_BG95\LLVM\4.0.3
	set TOOLCHAIN_PATH=%LLVMROOT%\bin
	set CC=clang
	set AR=llvm-ar
	set SDK_LIB_PATH=libs
	set INC_BASE=..\..\common\include
	set AZURE_SDK_SRC_PATH=..\azure-iot-sdk-c
	set AZURE_SDK_INC_PATH=..\azure-iot-sdk-c
	set AZURE_PORT_INC_PATH=..\porting-layer
	set SDK_OUTPUT_PATH=..\azure-iot-sdk-c\buildqc
	set SDK_LIB_NAME=azure_sdk.lib

For `build_azure_sdk_port_lib_llvm.bat`:

	set LLVMROOT=C:\Quectel_BG95\LLVM\4.0.3
	set TOOLCHAIN_PATH=%LLVMROOT%\bin
	set CC=clang
	set AR=llvm-ar
	set SDK_LIB_PATH=libs
	set INC_BASE=..\..\common\include
	set AZURE_PORT_SRC_PATH = ..\porting-layer
	set AZURE_SDK_INC_PATH=..\azure-iot-sdk-c
	set AZURE_PORT_INC_PATH=..\porting-layer
	set PORT_OUTPUT_PATH=..\porting-layer\build
	set SDK_PORT_LIB_NAME=azure_sdk_port.lib

Run `build_azure_sdk_lib_llvm.bat` and `build_azure_sdk_port_lib_llvm.bat` and copy the generated file `azure_sdk_port.lib` and `azure_sdk.lib` to `C:\Quectel_BG95\Quectel_BG95_QuecOpen_SDK_Package_V1.0.1\libs`

## 3.2 Build Sample

There is iothub_convenience_sample for sending messages to IoT Hub and receiving messages from IoT Hub as well as handling device direct method. This sample supports different protocols. You can make modification to the samples with your choice of protocol before building the samples. Follow the below instructions to edit the samples before building: 

1.  Copy `iothub_convenience_sample.c` under `C:\Quectel_BG95\azure-iot-sdk-c\iothub_client\iothub_convenience_sample\` to `C:\Quectel_BG95\Quectel_BG95_QuecOpen_SDK_Package_V1.0.1\quectel\example\tcp_client\src\`  

2.  Find the following placeholder for IoT connection string:

        static const char* connectionString = "[device connection string]";

3.  Replace the above placeholder with device connection string.
    
4.  Find the `(void)printf` placeholder, replace all the placeholders with `TCP_UART_DBG`.

5.  Save your changes.

6.  Navigate to `C:\Quectel_BG95\Quectel_BG95_QuecOpen_SDK_Package_V1.0.1\build_demo.bat`, modify the .bat file as following and save your file:

		set TOOL_PATH_ROOT=C:\Quectel_BG95
		set TOOLCHAIN_PATH=%TOOL_PATH_ROOT%\LLVM\4.0.3\bin
		rem echo TOOLCHAIN_PATH is %TOOLCHAIN_PATH%
		set TOOLCHAIN_PATH_STANDARdS=%TOOL_PATH_ROOT%\LLVM\4.0.3\armv7m-none-eabi\libc\include
		set LLVMLIB=%TOOL_PATH_ROOT%\LLVM\4.0.3\lib\clang\4.0.3\lib
		set LLVMLINK_PATH=%TOOL_PATH_ROOT%\LLVM\4.0.3\tools\bin
		set PYTHON_PATH=C:\Python27\python.exe
		set DAM_INC_BASE=include
		set DAM_LIB_PATH=libs
		set DEMO_SRC_PATH=quectel\example


7.  Run the following command  to build the sample for Quectel_BG95 device.

		`.\build_demo.bat llvm tcp_client'
	
After build success, `oem_app_path.ini` and `quectel_demo_tcp_client.bin` file which loacted at `C:\Quectel_BG95\Quectel_BG95_QuecOpen_SDK_Package_V1.0.1\bin\` will be geneated.

<a name="Step-3-3-Run"></a>
## 3.3 Run and Validate the Samples

In this section you will run the Azure IoT client SDK samples to validate
communication between your device and Azure IoT Hub. You will send messages to the Azure IoT Hub service and validate that IoT Hub has successfully receive the data. You will also monitor any messages send from the Azure IoT Hub to client.

Copy `oem_app_path.ini` and `quectel_demo_tcp_client.bin` to dataX folder (ThreadX OS of Quectle BG95) by QPST tool. Then press **PWRKEY** to restart device. The sample runs automatically and start to send messages to Iothub after device initialized. Open Serial Port Utility and you can monitor the sample app's output information.

You can also send messages to Quectel BG95 device from [Azure Portal](https://ms.portal.azure.com/#home). The message can also be monitored by Serial Port Utility.

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
