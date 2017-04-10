---
platform: Yocto
device: Nucleus
language: Javascript
---

Run a simple JavaScript sample on Nucleus device running Yocto
===
---

# Table of Contents
 * [Introduction](#Introduction)
 * [Step 1: Prerequisites](#Prerequisites)
 * [Step 2: Prepare your Device](#PrepareDevice)
 * [Step 3: Build the Sample](#Build)
 * [Step 4: Run the sample](#Run)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect PasSy Gateway device running Yocto with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites
 
 You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Nucleus device.

<a name="PrepareDevice"></a>  
# Step 2: Prepare your Device
* Ensure your're running nodejs 4.0+ on your device. You can check your version by running: 
    ``` 
    node -v 
    ```
* Clone the [azure-iot-sdk-node](https://github.com/azure/azure-iot-sdk-node) repo to your home directory on the device.
    ```
    git clone https://github.com/Azure/azure-iot-sdk-node.git
    ```
* Navigate into the samples directory
    ```
    cd azure-iot-sdk-node/device/samples/
    ```
* Validate the source code by running 
    ```
    export IOTHUB_CONNECTION_STRING='<iothub_connection_string>'
    ```
    <iothub_connection_string> is your IoT Device Connection String from the first step.

<a name="Build"></a>    
# Step 3: Build the Sample

* Run the following commands to build:

    ```
    cd ~/azure-iot-sdk-node
    ```
    
    ```
    build/dev-setup.sh
    ```
    
    ```
    build/build.sh | tee LogFile.txt
    ```
    *This might take some time*
    
    ```
    cd ~/azure-iot-sdk-node/device/samples/
    ```

    **Note:** this will create a log file called ```LogFile.txt``` to log build outputs and test results.

* The default protocol is AMQP. To install, run:

    ```
    npm install azure-iot-device-amqp
    ```
    
    You'll also need: 

    * The HTTP protocol
            
        ```
        npm install azure-iot-device-http
        ```
            
    * The MQTT protocol
        
        ```
        npm install azure-iot-device-mqtt
        ```
            
* To Update the sample, run these commands on the device:

    ```
    cd ~/azure-iot-sdk-node/device/samples/
    ```
    
    ```
    nano simple_sample_device.js
    ```
    
    This will launch nano, which we will use to edit the .js file. 
    Find the line of code that reads
    
    ```
    var Protocol = require('azure-iot-device-amqp').Amqp;
    ```
    
    If you installed the additional protocols, uncomment the protocols you installed and want to use.
    
* Search for the line of code that reads

    ```
    var connectionString = "[IoT Device Connection String]";
    ```
    
    Remove the placeholder text and insert your IoT Hub Connection String between the quotations.
    
* Press ```CTRL+X``` and then ```y``` and then ```ENTER``` to write the file and quit nano.
* Run the following command

    ```
    npm link azure-iot-device
    ```
<a name="Run"></a>
# Step 4: Run the Sample
-   To run the sample, issue the following command:

    ```
    node ~/azure-iot-sdk-node/device/samples/simple_sample_device.js
    ```
-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.
-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md