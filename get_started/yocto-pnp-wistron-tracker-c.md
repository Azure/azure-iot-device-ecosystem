---
platform: yocto
device: wistron-tracker
language:c
---

Connect Wistron-tracker device to your Azure IoT Central Application
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

This document describes how to connect Wistron-tracker to Azure IoT Central application using the IoT plug and Play model. Plug and Play simplifies IoT by allowing solution developers to integrate devices without writing any device code. Using Plug and Play, device manufacturers will provide a model of their device to cloud developers to be integrated quickly into IoT Central or any solution built on the Azure IoT platform. IoT Plug and Play will be open to the community by way of a definition language and SDKs.

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process: 

-   [Azure Account](https://portal.azure.com)

-   [Azure IoT Hub Instance](https://docs.microsoft.com/en-us/azure/iot-hub/about-iot-hub)

-   [Azure IoT Hub Device Provisioning Service](https://docs.microsoft.com/en-us/azure/iot-dps/about-iot-dps)

-   Provide Network connectivity (Wifi, LAN) supported by the device

-   Its mandatory that the device code/software image is preinstalled in device to enable Plug and Play

Note: If the device code is not preinstalled following are the [options](#preparethedevice) to choose to enable the plug and play device

# Step 2: Device Connection Details.

In your Azure IoT Central application, select the Administration tab and select Device Connection. Make a note of the Scope ID and Primary key.

# Step 3: Prepare the Device.

1.  Setup the GPS settings. If You don't change them, the GPS will be run with the default value.

    i. Connect the device to your development machine using a USB cable.

    ii. Get the adb tool from <https://dl.google.com/android/repository/platform-tools-latest-windows.zip>.

    iii. Open the terminal and enter the folder, type the "adb shell ./nmea_test_app -h".

    iv. You can see the below screen and change the value you want.
    usage:

        -t <time out in sec> -i <tbf msec>
        -t:  Time out, Defaults: 100
        -d:  Delete all aiding data, Defaults: 0
        -i:  Interval. Time in milliseconds between fixes, Defaults: 1000
        -m:  nmea types subscription mask
                NMEA_MASK_GGA        (0x00000001) Enable GGA type
                NMEA_MASK_RMC        (0x00000002) Enable RMC type
                NMEA_MASK_GSV        (0x00000004) Enable GSV type
                NMEA_MASK_GSA        (0x00000008) Enable GSA type
                NMEA_MASK_VTG        (0x00000010) Enable VTG type
                NMEA_MASK_PQXFI      (0x00000020) Enable PQXFI type
                NMEA_MASK_PSTIS      (0x00000040) Enable PSTIS type
                NMEA_MASK_GLGSV      (0x00000080) Enable GLGSV type
                NMEA_MASK_GNGSA      (0x00000100) Enable GNGSA type
                NMEA_MASK_GNGNS      (0x00000200) Enable GNGNS type
                NMEA_MASK_GARMC      (0x00000400) Enable GARMC type
                NMEA_MASK_GAGSV      (0x00000800) Enable GAGSV type
                NMEA_MASK_GAGSA      (0x00001000) Enable GAGSA type
                NMEA_MASK_GAVTG      (0x00002000) Enable GAVTG type
                NMEA_MASK_GAGGA      (0x00004000) Enable GAGGA type
                NMEA_MASK_PQGSA      (0x00008000) Enable PQGSA type
                NMEA_MASK_PQGSV      (0x00010000) Enable PQGSV type
                NMEA_MASK_ALL        (0x0001FFFF) Enable ALL types

        -p:  Print NMEA strings, Defaults: 1
        -r:  Print position report strings
        -c:  count : 1
        -h:  print this help

2.  Connect the network. 

    There are two ways to connect the azure azure iot central by using I.sim card over 3G/4G or II.WiFi network.

    I. The sim card over 3G/4G, please following the document in the below link.
    **The Link**- <https://fs.wistron.com/?ShareToken=8192B8E94670F7B8D11E7C4C55B4C3C1D14D87A7>
     **Password**: `2F7g1Gyj`

    II. WiFi 

       i.  please download the `wifion.sh` file 
       <https://fs.wistron.com/?ShareToken=141C8994C37826C916FE86A7C36E3C1DA6A92642>
       **Password:** `G39RgR2c`

      ii. Change the settings in wifion.sh file
 
        wpa_cli -p /var/run/wpa_supplicant remove_network 0
        wpa_cli -p /var/run/wpa_supplicant ap_scan 1
        wpa_cli -p /var/run/wpa_supplicant add_network
        wpa_cli -p /var/run/wpa_supplicant set_network 0 ssid '"KH_RD"' --> change your AP name.
        wpa_cli -p /var/run/wpa_supplicant set_network 0 key_mgmt WPA-PSK --> change your AP key security method
        wpa_cli -p /var/run/wpa_supplicant set_network 0 psk '"MR00o@P"' --> change your AP password
        wpa_cli -p /var/run/wpa_supplicant enable_network 0
  
      iii. push the file into the device and execute the following command.

        #chang local time
        adb shell date 110510102018 --> MMDDtime20YY

        #push file to device
        adb push wifion.sh /bin
        adb shell
        ./bin/wifion.sh
  
        # wait 3~5 sec
        dhcpcd wlan0

        # wait 3~5 sec
        #Try ping 8.8.8.8
        Ping 8.8.8.8

        #execute the application
        ./sensorbox_app "IoT Hub device connection string"

    The Wistron-tracker can connect to IoT hub and then starts sending data.
  
# Step 4: Integration with IoT Central

Please download the document and you can see the example data showed by IoT central.
<https://fs.wistron.com/?ShareToken=F2C6A22C5BCF9A1523BFF15C2AE0CC6B96100ABE>
**Password:** `mdSF2Y3M`

# Step 5: Additional Links

Please refer to the below link for additional information for Plug and Play 

-   [Blog](https://azure.microsoft.com/en-us/blog/iot-plug-and-play-is-now-available-in-preview/)
-   [FAQ](TBD) 
-   [Plug and Play C SDK](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) 
-   [Plug and Play Node SDK](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview)
-   [Plug and Play Definitions](https://github.com/Azure/IoTPlugandPlay)