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

**Note:** If the device code is not preinstalled following are the [options](#preparethedevice) to choose to enable the plug and play device

# Step 2: Device Connection Details.

In your Azure IoT Central application, select the Administration tab and select Device Connection. Make a note of the Scope ID and Primary key.

# Step 3: Prepare the Device.

There are two ways to connect the azure azure iot central by using 1.sim card over 3G/4G or 2.WiFi network.

1.  The sim card over 3G/4G, please following the document in the below link.
The Link- <https://fs.wistron.com/?ShareToken=3E625417D5943B238E786359B9DC9EF9BEA78ECA>
          Password: 5f473cWS
2.  WiFi 

    i.  please download the wifion.sh file
    <https://fs.wistron.com/?ShareToken=8620796F10320A443BFC397498434FAC93AA7E14>

    Password: p4264vXx

    ii. Change the settings in `wifion.sh` file

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
<https://fs.wistron.com/?ShareToken=DF1C0E0F70B67D29C9C3C0CE56049E5EBCD9E62B>

Password: 2753J0fM

# Step 5: Additional Links

Please refer to the below link for additional information for Plug and Play 

-   [Blog](https://azure.microsoft.com/en-us/blog/iot-plug-and-play-is-now-available-in-preview/)
-   [FAQ](TBD) 
-   [Plug and Play C SDK](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) 
-   [Plug and Play Node SDK](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview)
-   [Plug and Play Definitions](https://github.com/Azure/IoTPlugandPlay)