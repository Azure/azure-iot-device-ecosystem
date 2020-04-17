---
platform: linux
device: R3000 Router
language: c
---

Run a simple C sample on R3000 Router running linux
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

The following document describes the process of connecting an App of R3000 to Azure IoT Hub.This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Step-1-Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:
-   ROS C SDK which contain the azure-iot-sdk-c
- ROS router (R2000, R3000, ...)
- Centos 6.7 or newer 

<a name="Step-2-PrepareDevice"></a>
# Step 2: Prepare your Device
-  Connect the R3000 Standard ethernet cable.
-  Waiting for R3000 Standard connect to the cellular network.

<a name="Step-3-Build"></a>
# Step 3: Build and Run the sample

## Setup the development environment

- Enter the Centos OS, extract the SDK to any place, for example:

``` 
$ tar jxf ROS-SDK-r3000-XX.tar.bz2 /opt
```

- Add source code into application directory. Sample code was copy from the azure-iot-sdk-c.
```
 # ls application/iothub_client_sample_mqtt/src/ -l
    certs.c
    certs.h
    iothub_client_sample_mqtt.c
    iothub_client_sample_mqtt.h
```
- Open the file **application/iothub_client_sample_mqtt/src/iothub_client_sample_mqtt.c** in a text editor (for example Notepad++)
- Locate the following code in the file:
```
static const char* connectionString = "[device connection string]";
```
- Replace "[device connection string]" with the device connection string you noted [earlier](#beforebegin). Save the changes.
- Add Makefile of iothub_client_sample_mqtt, it must define the LIBS with **-liothub_client -liothub_client_mqtt_transport -lumqtt -laziotsharedutil**
- Add **package/iothub_client_sample_mqtt/Makefile**, this will setup the rule to build the iothub_client_sample_mqtt App and other depends.

 <a name="build"></a>
## Build the sample

- Enter top directory of ROS-SDK.
- Run the **make package/iothub_client_sample_mqtt/install**
```
make package/iothub_client_sample_mqtt/install
```
- After build successfully we can get the r3000-iothub_client_sample_mqtt-VERSION.rpk

<a name="deploy"></a>
## Deploy the sample

- Open the R3000 Standard Web App certer, install r3000-iothub_client_sample_mqtt-VERSION.rpk
- Reboot device.

<a name="run"></a>
## Run the sample

- Run the sample **iothub_client_sample_mqtt**

***Note: To send a command to the device from iothub-explorer or DeviceExplorer, the command should be like {"Name":"TurnFanOff","Parameters":{}}***
