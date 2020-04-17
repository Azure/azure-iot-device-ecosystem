---
platform: yocto linux
device: pumpkin i300 evk smart audio edition
language: python
---

Run a simple python sample on Pumpkin i300 EVK Smart Audio Edition device running Yocto Linux. 
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Manual Test for Azure IoT Edge on device](#Manual)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect Pumpkin i300 EVK Smart Audio Edition device running Yocto Linux with Azure IoT Edge Runtime pre-installed and Device Management. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and Deploy client component to test device management capability 

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   [Sign up to IOT Hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Add the Edge Device](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux)
-   [Add the Edge Modules](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux#deploy-a-module)
-   Pumpkin i300 EVK Smart Audio Edition. 

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   The device comes with all the files and configurations needed to run IoT Edge. However, you do need to do some device specific configurations to make sure that the device works with IoT Edge. For that you need to do the following: 

<a name="Network Setup"></a>
# Step 2.1: Setup the network

You can connect the device to a network using two methods: 

1.  Ethernet Cable: Simply connect your device to a router with internet connection using an ethernet cable. This should enable network on your device immediately.

2.  Wi-Fi: To setup the device to work with Wi-Fi you need the following pre-requsisites: 
   
       a. Wi-Fi SSID
       b. Wi-Fi PASSWORD

Once you have these ready, run the following commands on the device: 

    wpa_passphrase SSID PASSWORD >> /etc/wpa_supplicant/wpa_supplicant-wlan0.conf

    wpa_supplicant -i wlan0 -B -c /etc/wpa_supplicant/wpa_supplicant-wlan0.conf

You should have network connectivity after this step. Confirm that you are able to access the internet before proceeding. 

<a name="Connection String"></a>
# Step 2.2: Adding a connection string

-   Login to the Azure Portal and retrieve your device's connection string. 

-   On the device, add the connection string to the following file: 

        /etc/iotedge/config.yaml

-   After you have added the file to the device you should be able to start the IoT Edge Daemon with the following command: 

        iotedged -c /etc/iotedge/config.yaml`

**Note:** Make sure you specify the configuration file to use otherwise the IoT Edge Daemon will fail to start.    

<a name="Manual"></a>
# Step 3: Manual Test for Azure IoT Edge on device

This section walks you through the test to be performed on the Edge devices running the Linux operating system such that it can qualify for Azure IoT Edge certification.

## Edge RuntimeEnabled

**Details of the requirement:**

The following components come pre-installed or at the point of distribution on the device to customer(s):

-   Azure IoT Edge Security Daemon
-   Daemon configuration file
-   Moby container management system
-   A version of `hsmlib` 

*Edge Runtime Enabled:*

**Check the iotedge daemon command:** 

Open the command prompt on your IoT Edge device , confirm that the Azure IoT edge Daemon is under running state

    systemctl status iotedge

 ![](./media/ArtiGO/Capture.png)

Open the command prompt on your IoT Edge device, confirm that the module deployed from the cloud is running on your IoT Edge device

    sudo iotedge list

 ![](./media/ArtiGO/iotedgedaemon.png) 

On the device details page of the Azure, you should see the runtime modules - edgeAgent, edgeHub and tempSensor modueles are under running status

 ![](./media/ArtiGO/tempSensor.png)

  
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md