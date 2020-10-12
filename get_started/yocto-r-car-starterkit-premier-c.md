---
platform: yocto linux
device: r-car
language: c
---

Run a simple C sample on R-Car Starter Kit Premier device running Yocto
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

This document describes how to connect R-Car Starter Kit board running Yocto with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   [R-Car Starter Kit Premier/Pro][lnk-R-Car-SK]

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

## 2.1 Install required packages

-   **Ubuntu and Debian**

		$ sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \
     	 build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
     	 xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev \
     	 pylint3 xterm

-   **Fedora**

		$ sudo dnf install gawk make wget tar bzip2 gzip python3 unzip perl patch \
    	 diffutils diffstat git cpp gcc gcc-c++ glibc-devel texinfo chrpath \
     	 ccache perl-Data-Dumper perl-Text-ParseWords perl-Thread-Queue perl-bignum socat \
    	 python3-pexpect findutils which file cpio python python3-pip xz python3-GitPython \
    	 python3-jinja2 SDL-devel xterm rpcgen

-   Refer to [Yocto Project Quick Start][yocto-quick-start] for more information.

## 2.2 Building the image for R-Car Starter Kit Premier

### 2.2.1 Create a directory and switch to it

-   **Warning!** Yocto builds require a lot of disk space (up to 100 GB). Make sure you have got enough before starting the build.

		mkdir build
		cd build
		export WORK=`pwd`

### 2.2.2 Clone and checkout Yocto layers
-   Clone and checkout Yocto layers by using repo command.

		cd $WORK
		repo init -u https://github.com/tkomagata/yocto_manifests.git -m iotedge/rcar-gen3/dunfell_v4.1.0.xml
		repo sync
		TEMPLATECONF=$PWD/meta-iotedge/conf/machine/{h3ulcb or m3ulcb}/bsp/ \
			source poky/oe-init-build-env rcar-build

### 2.2.3 Modify local.conf (Optional)

-   Some settings can be set by modifying local.conf

		cd $WORK/rcar-build
		vi conf/local.conf

-   Uncomment and set timezone if necessary.

		# Set Tinezone (Optional)
		#DEFAULT_TIMEZONE ="UTC"

-   Uncomment and set the connection String.<br>
Its value is set in /etc/iotedge/config.yaml.

		# Set the connection String (Optional, NOT USE for production)
		#IOTEDGE_DEVICE_CONNECTION_STRING = ""

-   Uncomment and set the values if necessary.

		# Set options to generate certificates (Optional, NOT USE for production)
		#  https://aka.ms/iotedge-prod-checklist-certs
		#  https://github.com/Azure/iotedge/tree/master/tools/CACertificates
		#DEFAULT_VALIDITY_DAYS = "30"
		#ROOT_CA_PASSWORD = "1234"
		#INTERMEDIATE_CA_PASSWORD = "1234"
		#FORCE_NO_PROD_WARNING = "true"

-   Add following lines if azure-iot-sdk-c is compiled on the device based on [Step.3](#Build).<br>
These components are required for the compile.

		IMAGE_INSTALL_append = " nano cmake git"
		EXTRA_IMAGE_FEATURES = "debug-tweaks dev-pkgs tools-sdk"

### 2.2.4 Building images

-   Start bitbake

		cd $WORK/rcar-build
		bitbake core-image-minimal

-   Building image can take up to a few hours depending on your host system performance.<br>
After the build has been completed successfully, you should see the output similar to:

		NOTE: Tasks Summary: Attempted 3801 tasks of which 31 didn't need to be rerun and all succeeded.

	and the command prompt should return.  
	Bitbake has generated all the necessary files in ./tmp/deploy/images directory.

## 2.3 Setup the board

### 2.3.1 Flashing firmware
The instructions to flash firmware is on the [elinux: H3SK#Flashing_firmware][H3SK#Flashing_firmware] and firmwares are created to **$WORK/rcar-build/tmp/deploy/images/h3ulcb/*.srec** by [Step 2.2](#BuildingImages).  
Then, if this is a first time for you to use the R-Car Starter Kit, please refer to [elinux: H3SK#Quick_Start_How_To][H3SK#Quick_Start_How_To] too.

### 2.3.2 Preparing microSD card
The instructions to prepare SD card is on the [elinux: Loading kernel and rootfs via eMMC/SD card][Gen3-SD-Boot] and firmwares are created to **$WORK/rcar-build/tmp/deploy/images/h3ulcb/*. by [Step 2.2](#BuildingImages). 

### 2.3.3 Configure U-Boot to boot from SD card
The instructions to set U-Boot environment variables [elinux: Configure U-Boot to boot from SD card][Gen3-u-boot-SD].

<a name="Build"></a>
# Step 3: Build and Run the sample

## 3.1 Load the Azure IoT bits and prerequisites on device

-   Open a PuTTY (or Teraterm, Picocom, and etc) session and connect to the device.

-   Download the SDK to the board by issuing the following command in PuTTY:

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Verify that you now have a copy of the source code under the
    directory ~/azure-iot-sdk-c.

## 3.2 Build the samples

There are two samples one for sending messages to IoT Hub and another for receiving messages from IoT Hub. Both samples supports different protocols. You can make modification to the samples with your choice of protocol before building the samples. By default the samples will build for AMQP protocol.  Follow the below instructions to edit the samples before building:

### 3.2.1 Send Telemetry to IoT Hub Sample:

-   Open the telemetry sample file in a text editor

		nano azure-iot-sdk-c/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample.c     

-   Find the following placeholder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string.

-   Find the following place holder for editing protocol:

		// The protocol you wish to use should be uncommented
		//                                                                        
		#define SAMPLE_MQTT                                                       
		//#define SAMPLE_MQTT_OVER_WEBSOCKETS                                     
		//#define SAMPLE_AMQP                                        
		//#define SAMPLE_AMQP_OVER_WEBSOCKETS                
		//#define SAMPLE_HTTP                                

-   Please uncomment the protocol that you would like to test with and comment other protocols. If testing for multiple protocols, please repeat above step for each protocol.

-   Save your changes by pressing Ctrl+O and when nano prompts you to save it as the same file, just press ENTER.

-   Press Ctrl+X to exit nano.

### 3.2.2 Send message from IoT Hub to Device Sample:

-   Open the telemetry sample file in a text editor

	 	nano azure-iot-sdk-c/iothub_client/samples/iothub_ll_c2d_sample/iothub_ll_c2d_sample.c

-   Follow same steps 1-7 as above to edit this sample.

### 3.2.3 Build the samples:

-   Build the SDK using following command. If you are facing any issues during build.

        ./azure-iot-sdk-c/build_all/linux/build.sh | tee LogFile.txt

    ***Note:*** *LogFile.txt in above command should be replaced with a file name where build output will be written.*

    *build.sh creates a folder called "cmake" under "~/azure-iot-sdk-c/". Inside "cmake" are all the results of the compilation of the complete software.*

## 3.3 Run and Validate the Samples

In this section you will run the Azure IoT client SDK samples to validate
communication between your device and Azure IoT Hub. You will send messages to the Azure IoT Hub service and validate that IoT Hub has successfully receive the data. You will also monitor any messages send from the Azure IoT Hub to client.

### 3.3.1 Send Device Events to IOT Hub:

-   Run the sample by issuing following command.    

		azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample


### 3.3.2 Receive messages from IoT Hub

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
[lnk-setup-iot-hub]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md
[lnk-manage-iot-hub]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md
[lnk-R-Car-SK]: https://www.renesas.com/us/en/solutions/automotive/adas/solution-kits/r-car-starter-kit.html
[yocto-quick-start]: https://www.yoctoproject.org/docs/2.6/brief-yoctoprojectqs/brief-yoctoprojectqs.html#packages
[H3SK#Flashing_firmware]: https://elinux.org/R-Car/Boards/H3SK#Flashing_firmware
[H3SK#Quick_Start_How_To]: https://elinux.org/R-Car/Boards/H3SK#Quick_Start_How_To
[Gen3-SD-Boot]: https://elinux.org/R-Car/Boards/Yocto-Gen3/v3.21.0#Loading_kernel_and_rootfs_via_eMMC.2FSD_card
[Gen3-u-boot-SD]: https://elinux.org/R-Car/Boards/Yocto-Gen3/v3.21.0#Configure_U-Boot_to_boot_from_SD_card
