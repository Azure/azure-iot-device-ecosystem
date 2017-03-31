---
platform: OpenWrt
device: LinkIt Smart 7688 Duo
language: c
---

Run a simple C sample on LinkIt Smart 7688 Duo device running OpenWrt
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Tips](#tips)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect LinkIt Smart 7688 Duo device running OpenWrt with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-    1.1 Set up the ubuntu x86 machine (for cross compiling)
    1.2 Create an Azure IoT hub, install DeviceExplorer tool, add a IoT devicce
    1.3 Hardware - LinkIt Smart 7688 Duo - https://www.seeedstudio.com/LinkIt-Smart-7688-Duo-p-2574.html



<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
   2.1 Use PUTTY tool connect LinkIt Smart 7688 Duo to a usable network, can make reference to this wiki - http://wiki.seeed.cc/LinkIt_Smart_7688_Duo/


<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
  3.1 Build SDK on ubuntu machine
    
        3.1.1 Install cmake and gcc
        
            sudo add-apt-repository ppa:george-edison55/cmake-3.x      # cmake ppa
            sudo add-apt-repository ppa:ubuntu-toolchain-r/test        # gcc ppa
            sudo apt-get update
            sudo apt-get install cmake
            sudo apt-get install gcc-4.9 g++-4.9
            sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 60 --slave /usr/bin/g++ g++ /usr/bin/g++-4.9
			
        3.1.2 Clone github repository
		
			git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git
		
		3.1.3 Navigate to the folder azure-iot-sdk-c/build_all/arduino, and do some updates
		
			3.1.3.1 Update the sdk_repo references in the setup_sdk.sh
			
				sdk_repo="http://download.labs.mediatek.com/MediaTek_LinkIt_Smart_7688_Openwrt_sdk_Linux"
				sdk_repo_x86="http://download.labs.mediatek.com/MediaTek_LinkIt_Smart_7688_Openwrt_sdk_Linux"
				sdk_repo_x64="http://download.labs.mediatek.com/MediaTek_LinkIt_Smart_7688_Openwrt_sdk_Linux"
			
			3.1.3.2 Update the following references in the Makefile.iot
			
				...
				$(CP) ./src/azure-iot-sdk-c/* $(PKG_BUILD_DIR)/
				...
				CC="$(TOOLCHAIN_DIR)/bin/mipsel-openwrt-linux-gcc" \
				GCC="$(TOOLCHAIN_DIR)/bin/mipsel-openwrt-linux-gcc" \
				CXX="$(TOOLCHAIN_DIR)/bin/mipsel-openwrt-linux-g++" \
				...
				
        3.1.4 Build sdk and sample
        
            cd azure-iot-sdk-c/build_all/arduino
            sudo ./setup.sh
            sudo ./build.sh
        
        3.1.5 Use "scp" command copy files to the boad
		
			scp ~openwrt/sdk/build_dir/target-mipsel_24kec+dsp_uClibc-0.9.33.2/azure-iot-sdks-1/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp root@mylinkit.local:/tmp
			
			or
			
			scp ~openwrt/sdk/build_dir/target-mipsel_24kec+dsp_uClibc-0.9.33.2/azure-iot-sdks-1/iothub_client/samples/iothub_client_sample_http/iothub_client_sample_http root@mylinkit.local:/tmp
			
	3.2 Run the command on board
	
		3.2.1 Install libuuid
		
			opkg update
			opkg install libuuid
		
		3.2.2 Change the files as a executable file
		
			chmod +x ./iothub_client_sample_amqp
			chmod +x ./iothub_client_sample_http
   
    3.3 Send Device Events to IoT Hub
            
        3.3.1 Run the sample and send message on board
			
			./tmp/iothub_client_sample_amqp 
			
			or 
			
			./tmp/iothub_client_sample_http

    3.4 Receive messages from IoT Hub

        3.4.1 Use DeviceExplorer send message
		
			"Message amqp send to MT7688" or "Message http send to MT7688"
			
<a name="tips"></a>
# Tips

- If you just want to build the serializer samples, run the following commands:

  ```
  cd ./c/serializer/build/linux
  make -f makefile.linux all
  ```

[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../../setup_iothub.md
[lnk-manage-iot-hub]: ../../manage_iot_hub.md
