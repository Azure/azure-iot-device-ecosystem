---
platform: open wrt
device: servicegate v2
language: c
---

Run a simple C sample on ServiceGate v2 device running Open WRT
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Tips](#tips)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect the ServiceGate v2 device running Open WRT with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   The ServiceGate v2 device.
-   The `Azure-ServiceGate` Open WRT package or an `Azure-ServiceGate` repository clone.
-   A host system to build the package with.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

## Use the `Azure-ServiceGate` repository.

-   Follow the instructions as described in the `README.md` file in the root of the git repository.

## Use the `Azure-ServiceGate` Open WRT package.

>   NOTE: You must have the Open WRT package included in your SDK Root. If you do not have it included, please follow the instructions as mentioned above under the 'Use the `Azure-ServiceGate` repository.' header.

-   Go to the Open WRT SDK root (the `/source` folder in the `firmware-v1701` repository preferred).
-   Run `make menuconfig` in your Terminal.
-   Include the `azure-service-gate` package, which can be found under the `Network` category.
-   Exit the menuconfig menu and run `make` in your Terminal to embed the package in your new ServiceGate image.
-   Load the firmware image on the ServiceGate. How to exactly do this, depends on the situation. Contact a developer for specific instructions.

<a name="Build"></a>
# Step 3: Build and Run the sample

## 3.1 Load the Azure IoT bits and prerequisites on the host system

-   Open a PuTTY session and connect to the host system.

-   Install the prerequisite packages by issuing the following commands from the command line on the device. Choose your commands based on the OS running on your device.

    **Debian or Ubuntu**

        sudo apt-get update

        sudo apt-get install -y curl uuid-dev libcurl4-openssl-dev build-essential cmake git

    **Fedora**

        sudo dnf check-update -y

        sudo dnf install uuid-devel libcurl-devel openssl-devel gcc-c++ make cmake git

    **Any Other Linux OS**

        Use equivalent commands on the target OS

    ***Note:*** *This setup process requires cmake version 2.8.12 or higher.* 
    
    *You can verify the current version installed in your environment using the  following command:*

        cmake --version

    *This library also requires gcc version 4.9 or higher. You can verify the current version installed in your environment using the following command:*
    
        gcc --version 

    *For information about how to upgrade your version of gcc on Ubuntu 14.04, see <http://askubuntu.com/questions/466651/how-do-i-use-the-latest-gcc-4-9-on-ubuntu-14-04>.*
    
-   Download the SDK to the board by issuing the following command in PuTTY:

        git clone --recursive http://git.local/jan/Azure-ServiceGate.git

-   Verify that you now have a copy of the source code under the
    directory ~/Azure-ServiceGate.

<a name="Step-3-2-Build"></a>
## 3.2 Build the samples

-   Open the file **src/main.cpp** in a text editor (for example nano)
-   Locate the following code in the file:

```
MQTT::Init("[device connection string]");
```

-   Replace "[device connection string]" with the device connection string you noted [earlier](#beforebegin). Save the changes.
-   Run ```build.sh --remote``` in your Terminal.
-   make package/azure-service-gate/{clean,compile,install} V=s

<a name="deploy"></a>
## 3.3 Deploy the sample

-   Open a shell and navigate to the installed OpenWRT SDK folder.
-   Transfer the sample executable. (You also can use tftp to copy the executable to the ServiceGate. Please transfer `Azure` to the ServiceGate.)


```
scp <location-to-Azure-executable> <user>@router.local:~
```


## 3.4 Make sure the certificates are installed

On the ServiceGate v2 device, install the ca-certificates package like below(Also you can use curl tool to download the ipk.):

    wget https://downloads.openwrt.org/snapshots/trunk/ar71xx/generic/packages/base/ca-certificates_20161130_all.ipk --no-check-certificate
    opkg install ca-certificates_20161130_all.ipk

You might get an error message at this step(return code 127), but the certificates will be installed.

***Note: The certificate name mention above may change when newer version of certificate is released. If you get a 404 error while downloading the certificate file, please double check the CA certificate name under the base path [here](https://downloads.openwrt.org/snapshots/trunk/ar71xx/generic/packages/base) and update the certificate path accordingly.***

There are two samples one for sending messages to IoT Hub and another for receiving messages from IoT Hub. Both samples supports different protocols. You can make modification to the samples with your choice of protocol before building the samples. By default the samples will build for AMQP protocol.  Follow the below instructions to edit the samples before building: 

<a name="Step-3-5-Run"></a>
## 3.5 Run and Validate the Samples

1. Connect to the ServiceGate using PuTTY or SSH.

2. Run the `Azure` executable:

```
user@ServiceGate:~# ./Azure
```

If the executables does not show any errors, you have successfully built the Azure-ServiceGate package!

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
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

