---
platform: raspbian
device: 3GPI
language: c
---

Run a simple C sample on 3GPI device
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

This document describes the process of setting up a [Raspberry Pi 2](https://www.raspberrypi.org/) device to connect to an Azure IoT hub with [3GPI](http://www.mechatrax.com/products/3gpi).This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Step-1-Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:
-   Computer with Git client installed and access to the
    [azure-iot-sdk-c](https://github.com/Azure/azure-iot-sdk-c) GitHub public repository.
-   Terminal emulator on your desktop computer, such as [Tera Term](https://osdn.jp/projects/ttssh2/) or [PuTTY](http://www.putty.org/), so you can access the command line on the Raspberry Pi.
- Required hardware:  
  - [Raspberry Pi 2](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/)  
  - [3GPI](http://www.mechatrax.com/products/3gpi)  
	  Includes micro SD, mini-USB cable, AC adapter, and 3G antenna.
  - USB-to-Serial cable, such as [TTL-232R-RPI](http://www.ftdichip.com/Products/Cables/RPi.htm).  
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

<a name="Step-2-PrepareDevice"></a>
# Step 2: Prepare your Device
-  Attach the 3G antenna to the 3GPI.
-  Stack the 3GPI on the Raspberry Pi 2.
-  Connect the 3GPI and Raspberry Pi 2 using the mini-USB cable.
-  Connect the Raspberry Pi 2 into your computer using the USB-to-Serial cable.  
   For more information, see http://elinux.org/RPi_Serial_Connection
-  Insert the SD to the Raspberry Pi 2.
-  Plug the AC adapter into the 3GPI from the outlet.
-  When prompted, log in with username **pi**, and password **raspberry**.

<a name="Step-3-Build"></a>
# Step 3: Build and Run the sample

<a name="Load-the-Azure-IoT-bits-and-prerequisites"/>
## Load the Azure IoT bits and prerequisites

Run the following commands in the terminal window connected to your Raspberry Pi.

- Download the Azure IoT device SDK to your Raspberry Pi:

  ```
  git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git
  ```

- Confirm that you now have a copy of the SDK under the directory ./azure-iot-sdk-c.
Then cd to the directory:
  ```
  cd azure-iot-sdk-c
  ```

- Prepare your environment by running. Answer **y** when you are prompted to install the additional components needed to run the samples:
  ```
  sudo ./c/build_all/linux/setup.sh
  ```

<a name="Build-the-Azure-client"/>
## Update the Send sample

- Before performing these steps, you'll need the following prerequisites:

	- Device hub name.
	- Device ID (from Device Explorer).
	- Device key (from Device Explorer).

- Now, build the sample:

	- On the board, run the following command:

			nano azure-iot-sdk-c/serializer/samples/simplesample_http/simplesample_http.c

	- This launches a console-based text editor. Scroll down to the connection information.

	- Copy your saved credentials, and paste them here (by right-clicking).

	- Save your changes by pressing **Ctrl+O**, and when nano prompts you to save it as the same file, just press **ENTER**.

	- Press **Ctrl+X** to exit nano.

## Build the sample

- You can now build the SDK code using the following command.

		~/azure-iot-sdk-c/build_all/linux/build.sh

	**Note:** build.sh creates a folder called "cmake" in your home folder. Inside "cmake" are all the results of the compilation of the complete software.

<a name="Run-the-samples"/>
## Run the samples

- Run the **simplesample_http** sample by issuing the following command:

        azure-iot-sdk-c/cmake/iotsdk_linux/serializer/samples/simplesample_http/simplesample_http

- Verify that the **Confirmation** messages show an **OK**. If not, then you may have incorrectly pasted the device hub connection information.

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
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
