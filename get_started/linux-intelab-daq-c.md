---
platform: linux
device: intelab daq
language: c
---

Run a simple C sample on INTELAB DAQ device running linux
===
---

# Table of Contents

-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Tips](#tips)


**About this document**

This document describes how to connect INTELAB DAQ device running linux with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a> 
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   INTELAB DAQ device.
-   Ubuntu PUTTY

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
-   2.1 INTELAB DAQ Device connection network
-   Device connection serial port and Open the putty  

        ./mnt/bin/wifi;
-   2.2 The time calibration
-   Device connection serial port and Open the putty  

        ntp.sh;


<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build MQTT and sample

-   Open a PuTTY session and connect to the device.

-   Install the prerequisite packages for the MQTT for C by issuing the following commands from the command line on your board:
***download openssl and mqtt***

    **Debian or Ubuntu**
    git clone https://github.com/openssl/openssl.git
    git clone https://github.com/eclipse/paho.mqtt.c.git
***build openssl and mqtt***
   Enter the corresponding directory
   ./configure
    make
   make install
 

-   Download the mqtt SDK for C to the board by issuing the following command on the board::

        telnet 192.168.8.1
        mount -t nfs -o nolock 192.168.8.101:/home/linux/nfs/rootfs /media
        cp ~/nfs/rootfs/*3as* MQTTAsync_publish /home
        
        



-   Find the following place holder for IoT connection string:

        static const char* connectionString = "[device connection string]";

-   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.


## 3.2 Send Device Events to IoT Hub:

-   Run the sample by issuing following command:
***run sample.***



    **If using MQTT protocol:**

        ~/home/MQTTAsync_publish

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

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
