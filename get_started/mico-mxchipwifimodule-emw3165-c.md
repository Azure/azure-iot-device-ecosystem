---
platform: MiCO
device: mxchip wifi module board(MiCOKit3239/MiCOKit3166)
language: c
---

Run a simple C sample on supported MiCOKit device running MiCO
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)

# Introduction

**About this document**

This document describes how to connect supported MiCOKit device running MiCO with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK using [azure-iot-c-sdk-mico](https://github.com/zhaojuntao/azure-iot-c-sdk-mico) on supported device
-	Validate the sample using secureCRT-serial-debug-tool and Device-Explorer-Twin

**About the MiCOKit**

MiCOKit board is ShangHai MXCHIP's development kits for developer and engineer to fast connect the iot internet.
Static state show:

-   MXCHIP's WiFi module picture

![](./media/Azure--iothubmico-setup/MXCHIP-EMW3239-E.jpg)

-   MXCHIP's development kits picture

![](./media/Azure--iothubmico-setup/MiCOKit_development_kit01.png)

![](./media/Azure--iothubmico-setup/MiCOKit_development_kit02.jpg)

<a name="Prerequisites"></a>
# Step 1: Prerequisites

Before executing any of the steps below, read through each process, step by step
to ensure end to end understanding.

You should have the following items ready before beginning the process:

-   Computer with GitHub installed and access to the
    [azure-iot-sdk-c](https://github.com/Azure/azure-iot-sdk-c) and [azure-iot-c-sdk-mico](https://github.com/zhaojuntao/azure-iot-c-sdk-mico) GitHub
    public repository.
-   Required hardware:
	-	Prepare [mxchip wifi module board](http://www.mxchip.com/product/wifi),for example MiCOKit3239.Please submit Sample application information on [mico.io](http://bbs.mico.io/),if you have not a mxchip wifi module board.
	-	Download and install [MiCoder IDE](http://developer.mico.io/downloads)
	-   USB Mini cable
	-	STLINK-v2/v1 or J-link v8/v9

***Note:*** *If you haven't contacted Microsoft about being an Azure Certified for IoT partner, please submit this [form](<https://catalog.azureiotsuite.com/>) first to request it and then follow these instructions.*

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Install the latest MiCoderIDE software on your computer by
following the instructions in the [MiCO开发者中心](http://developer.mico.io/docs/13).
-   When the installation process is complete, the MiCoderIDE software have cotain latest MiCO SDK.You need to add azureIotclient files to MiCOderIDE project explorer's demos directory, you can directly copy [“demos\application\azure\azure_iothub_mqtt_demo” files](https://github.com/zhaojuntao/azure-iot-c-sdk-mico/tree/master/demos/application/azure/azure_iothub_mqtt_demo) here.And you need to add [“libraries\protocols\azure” files](https://github.com/zhaojuntao/azure-iot-c-sdk-mico/tree/master/libraries/protocols/azure) to MiCOderIDE project explorer's libraries directory.

  ![](./media/Azure--iothubmico-setup/micoderIDE01.png)

-	[Regist azure.microsoft account](https://azure.microsoft.com/zh-cn/free/) to get free account for iothub.
-	Add iothub service in azure cloud control center.
-	Install [Azure tool(Azure CLI)](https://docs.microsoft.com/zh-cn/azure/iot-hub/iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32) on your computer.
-	Click here to download and install DeviceExplorer，you will register your device using DeviceExplorer. Fllowing [here](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/iotcertification/iot_certification_port_c_libraries_other_platforms/iot_certification_port_c_libraries_other_platforms.md) to study how to regist your device.
-	When have you regested your device, you can get DEVICE ID and connect string-primary key in your devices list. Copy the connect string-primary key and pasted on micoderIDE's "/demos/azureIotclient/mqtt/azure_mqtt_client.c".(static const char* connectionString = "xxx";)

  ![](./media/Azure--iothubmico-setup/micoderIDE02.png)

<a name="Build"></a>
# Step 3: Build SDK and Run the sample

 -	In micoderIDE, using "azureIotclient.mqtt@MK3165 total download run JTAG=stlink-v2" command to compile and download to the board.
 -	In this section you will run the Azure IoT client SDK samples and DeviceExplorer to validate communication between your device and Azure IoT Hub. You will send messages to the Azure IoT Hub service and validate that IoT Hub has successfully receive the data.

[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../../setup_iothub.md
[lnk-manage-iot-hub]: ../../manage_iot_hub.md




