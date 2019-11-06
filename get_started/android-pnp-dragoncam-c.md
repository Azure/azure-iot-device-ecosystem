---
platform: android
device: dragoncam
language: c
---

Connect DragonCam device to your Azure IoT Central Application
===

---
# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Device Connection Details](#Deviceconnectiondetails)
-   [Step 3: Prepare the Device](#Preparethedevice)
-   [Step 4: Integration with IoT Central](#IntegrationwithIoTCentral)
-   [Step 5: Additional Links](#AdditionalLinks)

<a name="Introduction"></a>
# Introduction 

**About this document**

This document describes how to connect DragonCam to Azure IoT Central application using the IoT plug and Play model. Plug and Play simplifies IoT by allowing solution developers to integrate devices without writing any device code. Using Plug and Play, device manufacturers will provide a model of their device to cloud developers to be integrated quickly into IoT Central or any solution built on the Azure IoT platform. IoT Plug and Play will be open to the community by way of a definition language and SDKs.

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process: 

-   [Azure Account](https://portal.azure.com)
-   [Azure IoT Hub Instance](https://docs.microsoft.com/en-us/azure/iot-hub/about-iot-hub)
-   [Azure IoT Hub Device Provisioning Service](https://docs.microsoft.com/en-us/azure/iot-dps/about-iot-dps)

Network connectivity:**Wifi**

<a name="Deviceconnectiondetails"></a>
# Step 2: Device Connection Details.

**Prerequisite** 

Install ADB on linux first
Open terminal and install adb by following command

    sudo apt-get install android-tools-adb

Connect device with usb to your PC and check device connection by following command in terminal

    adb devices

![](https://www.dropbox.com/s/uvxsyvyeskzfww1/deviceConnection.png?raw=1)

Download latest version of Termux in link

    https://f-droid.org/packages/com.termux/

Install Termux for device by following command

    adb install /path/to/termux/apk

Install Vysor in Google Chrome browser 

Click link below

   <https://vysor.io/download/>

and click "Chrome" in page to install Vysor

![](https://www.dropbox.com/s/mny9f7htseefo1p/install_vysor.png?raw=1)

Connect device with usb and Vysor then open Termux on device as pic below

![](https://www.dropbox.com/s/07fyef64qygrddv/openVysor.png?raw=1)

![](https://www.dropbox.com/s/dzsk23yotijozcn/viewDevice.png?raw=1)

![](https://www.dropbox.com/s/fusesng5uy2x19s/connectDevice.png?raw=1)

![](https://www.dropbox.com/s/rh2mzl4zhjbyfaf/appPage.png?raw=1)

![](https://www.dropbox.com/s/spnpgfjxnh7uq9e/openTermux.png?raw=1)

Then install required packages in Termux by following command

    pkg install cmake
    pkg install openssl
    pkg install libcurl
    pkg install libuuid

Download azure-iot-sdk in your PC by 

    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview

And download code for device from link 

    https://www.dropbox.com/s/ztxw5hly52qq6h9/dragoncam.tar.gz?dl=0

Install keygen tool in PC

    npm i -g dps-keygen

Connect your device with PC by usb port,then put code in 'azure-iot-sdk-c' folder and push to device by

    adb push /path/of/iot-sdk /data/data/com.termux/files/home/

<a name="Preparethedevice"></a>
# Step 3: Prepare the Device.

**Launch termux app in device**

Add following line in end of CMakeLists.txt in iot sdk folder

    add_subdirectory(dragoncam/DragonCamDps)

In the same **azure-iot-sdk-c-pnp** folder, create a folder to contain the compiled app.

    mkdir cmake
    cd cmake

In the **cmake** folder you just created, run CMake to build the entire folder of Device SDK including the generated app code.

    cmake .. -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -Dskip_samples:BOOL=ON
    cmake --build .

To execute program

    cd dragoncam/DragonCamDps
    ./DragonCamDps

Code change can operate on device via termux or on your PC then push code to device.
Any code changes should be re-compile code.   

<a name="IntegrationwithIoTCentral"></a>
# Step 4: Integration with IoT Central

###4-1. Follow picture below to create device template

1.

![](https://www.dropbox.com/s/ax10ok60uin2200/step1.png?raw=1)

2.select custom

![](https://www.dropbox.com/s/aigvf8x5gvb96yk/step2.png?raw=1)

3.enter a template name

![](https://www.dropbox.com/s/8sjoblrmdrpws49/step3.png?raw=1)

4.

![](https://www.dropbox.com/s/jnty6eo9fcf1gre/step4.png?raw=1)

5.edit identity

![](https://www.dropbox.com/s/c4ov8o1u430t3hg/step5.png?raw=1)

6.

![](https://www.dropbox.com/s/1tddioa8xfwv0za/step6.png?raw=1)

7.add interface

![](https://www.dropbox.com/s/4k1zi00g39or4ka/step7.png?raw=1)

8.press import interface and select `EnvironmentalSensor.interface.json` download in step 2 

![](https://www.dropbox.com/s/wt5cq5v5r5nkrdd/step8.png?raw=1)

9.

![](https://www.dropbox.com/s/d99u2ihat0ug061/step9.png?raw=1)

10.Edit identity of the Environmental Sensor interface

![](https://www.dropbox.com/s/8ha1hvix2o3x0ru/step10.png?raw=1)

11.Select Add Interface, then Device Information to import the stanrdard DeviceInformation interface

![](https://www.dropbox.com/s/bqp3i88is0gcj38/step11.png?raw=1)

12.

![](https://www.dropbox.com/s/jqxzf5d7i8eh4ca/step12.png?raw=1)

##4.2 Connect device to iot central

![](https://www.dropbox.com/s/j3dpd7pmbjqj62q/step13.png?raw=1)

1. In your IoT Central application, go to the Administration page and select Device Connection.
Make a note of the Scope ID and Primary Key. We will need to paste the Scope ID to our Device Agent code later.

2. Open command prompt and run the following command to generate DPS symmetric key for the device:

        dps-keygen -di:{Device ID. Ex:dragoncam01} -mk:{Primary Key from IoT Central}

    The Device ID is a unique identification name for the device itself (different from the display name or ID of the device model/template). Use only use alphanumeric, lowercase, and may contain hyphens.
    Make a note on the Device ID you just inputted and generated DPS symmetric key. We will add this DPS information to our device agent.

3. Open main.c in your dragoncam project.

4. Specify parameters requested by the commented **TODO's** for your configuration.

        // TODO: Specify DPS scope ID if you intend on using DPS / IoT Central.
    static const char *dpsIdScope = "[DPS Id Scope]";
          
        // TODO: Specify symmetric keys if you intend on using DPS / IoT Central and symmetric key based auth.
    static const char *sasKey = "[DPS symmetric key]";
          
        // TODO: specify your device registration ID
    static const char *registrationId = "[Device ID from  2]";

Then recompile code and execute follow step 3 upon

Once device conneted to IoT Central ,telemetry data will show up IoT Central like picure below

![](https://www.dropbox.com/s/sxzbhudygz6ivyn/step14.png?raw=1)

![](https://www.dropbox.com/s/rjpydlrrmgdmpr6/DragonCamIotCentral.png?raw=1)

<a name="AdditionalLinks"></a>
# Step 5: Additional Links

Please refer to the below link for additional information for Plug and Play 

-   [Blog](https://azure.microsoft.com/en-us/blog/iot-plug-and-play-is-now-available-in-preview/)
-   [FAQ](TBD) 
-   [Plug and Play C SDK](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) 
-   [Plug and Play Node SDK](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview)
-   [Plug and Play Definitions](https://github.com/Azure/IoTPlugandPlay)
