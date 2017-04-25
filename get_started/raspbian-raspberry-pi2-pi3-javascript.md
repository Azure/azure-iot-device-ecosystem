---
platform: raspbian
device: raspberry pi2/pi3
language: javascript
---

Run a simple Node sample on Raspberry PI2/PI3 device running Raspbian
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Run sample that sends telemetry data to IoT Hub](#Sample)
	-   [Option 1: Use the development board, without sensors](#Device-Sample)
	-   [Option 2: Use the Raspberry 3 Azure IoT Starter Kit from Adafruit](#Kit01-Sample)
-   [Next steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to build and run the **simple_sample_http.js** Node.js sample application. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:
-   Computer with Git client installed and access to the
    [azure-iot-sdks](https://github.com/Azure/azure-iot-sdks) GitHub public repository.
-   [Prepare your development environment](node-devbox-setup.md).
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Make sure desktop is ready as per instructions given on [Prepare your development environment][lnk-setup-devbox].

<a name="Sample"></a>
# Step 3: Run sample that sends telemetry data to IoT Hub

<a name="Device-Sample"></a>
## Option 1: Use the development board, without sensors

-   Get the following sample files from https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples
    -   **package.json**
    -   **simple_sample_device.js**

-   Place the files in the folder of your choice on the target machine/device

-   Open the file **simple_sample_device.js** in a text editor.

-   Locate the following code in the file:

    ```
    var connectionString = '[IoT Device Connection String]';
    ```

-   Replace `[IoT Device Connection String]` with the connection string for your device. Save the changes.

-   From the shell or command prompt you used earlier to run the **iothub-explorer** utility, use the following command to receive device-to-cloud messages from the sample application (replace <device-id> with the ID you assigned your device earlier):

    ```
    node iothub-explorer.js monitor-events <device-id> --login "<iothub-connection-string>" 
    ```

-   Open a new shell or Node.js command prompt and navigate to the folder where you placed the sample files. Run the sample application using the following commands:

    ```
    npm install
    node simple_sample_device.js
    ```

-   The sample application will send messages to your IoT hub, and the **iothub-explorer** utility will display the messages as your IoT hub receives them.

### Experimenting with various transport protocols
The same sample can be used to test AMQP, AMQP over Websockets, HTTP and MQTT. In order to change the transport, uncomment whichever you want to evaluate in the `require` calls on top of the sample code and pass it to the call to Client.fromConnectionString() when creating the client.

### Debugging the samples (and/or your code)
[Visual Studio Code](https://code.visualstudio.com/) provides an excellent environment to write and debug Node.js code:
-   [Debugging with Visual Studio Code](./node-debug-vscode.md)

<a name="Kit01-Sample"></a>
## Option 2: Use the Raspberry 3 Azure IoT Starter Kit from Adafruit

We will use the following items from the kit:

- Raspberry Pi 3 development board 
- An assembled Adafruit BME280 temperature, pressure, and humidity sensor.
- A breadboard.
- 6 F/M jumper wires.
- A diffused 10-mm LED.

### Connect the sensors

Use the breadboard and jumper wires to connect an LED and a BME280 to Pi as follows. 

1.  **Fritzing diagram**

    ![The Raspberry Pi and sensor connection](../kits/getstarted/images/adafruit/raspberry-pi3/3_raspberry-pi-sensor-connection.png)

2.  **Wiring matrix**

    For sensor pins, use the following wiring:

    | Start (Sensor & LED)     | End (Board)            | Cable Color   |
    | -----------------------  | ---------------------- | ------------: |
    | VDD (Pin 5G)             | 3.3V PWR (Pin 1)       | White cable   |
    | GND (Pin 7G)             | GND (Pin 6)            | Brown cable   |
    | SCK (Pin 8G)             | I2C1 SDA (Pin 3)       | Orange cable  |
    | SDI (Pin 10G)            | I2C1 SCL (Pin 5)       | Red cable     |
    | LED VDD (Pin 18F)        | GPIO 24 (Pin 18)       | White cable   |
    | LED GND (Pin 17F)        | GND (Pin 20)           | Black cable   |

    Click to view [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.

3.  **Picture**

    After you've successfully connected BME280 to your Raspberry Pi, it should be like below image.

    ![Connected Pi and BME280](../kits/getstarted/images/adafruit/raspberry-pi3/4_connected-pi.jpg)

### Build and Run the sample

#### Clone sample application and install the prerequisite packages

1.  Use one of the following SSH clients from your host computer to connect to your Raspberry Pi.
    - [PuTTY](http://www.putty.org/) for Windows.
    - The built-in SSH client on Ubuntu or macOS.

2.  Clone the sample application by running the following command:

        git clone https//github.com/Azure-samples/iot-hub-node-raspberry-pi-clientapp

3.  Install all packages by the following command. It includes Azure IoT device SDK, BME280 Sensor library and Wiring Pi library.

        cd iot-hub-node-raspberry-pi-clientapp
        npm install

    > [!NOTE] 
    It might take several minutes to finish this installation process depending on your network connection.

#### Configure the sample application

1.  Open the config file by running the following commands:

        nano config.json

    ![Config file](../kits/getstarted/images/adafruit/raspberry-pi3/6_config-file.png)

    There are two items in this file you can configure. The first one is `interval`, which defines the time interval between two messages that send to cloud. The second one `simulatedData`,which is a Boolean value for whether to use simulated sensor data or not.

    If you **don't have the sensor**, set the `simulatedData` value to `true` to make the sample application create and use simulated sensor data.

2.  Save and exit by pressing Control-O > Enter > Control-X.

#### Run the sample application

1.  Run the sample application by running the following command:

        sudo node index.js '<your Azure IoT hub device connection string>'

    > [!NOTE] 
    Make sure you copy-paste the device connection string into the single quotes.

    You should see the following output that shows the sensor data and the messages that are sent to your IoT hub.

    ![Output - sensor data sent from Raspberry Pi to your IoT hub](../kits/getstarted/images/adafruit/raspberry-pi3/8_run-output.png)

<a name="NextSteps"></a>
# Next steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
-   [Save IoT Hub messages to Azure data storage](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
-   [Remote monitoring and notifications with ​​Logic ​​Apps](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)
-   [Device management with iothub-explorer](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-device-management-iothub-explorer)
    - This tutorial only works with the sample for the Raspberry Pi 3 kit from Adafruit


[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-setup-devbox]: node-devbox-setup.md

