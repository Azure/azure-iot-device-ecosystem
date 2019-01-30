---
platform: edge raspbian
device: mypi industrial iot integrator board
language: c
---

IoT Edge Device Management Components installation on MyPi Industrial IoT Integrator Board device running Raspbian
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Step-1-Prerequisites)
-   [Step 2: Prepare your Device](#Step-2-PrepareDevice)
-   [Step 3: Install the MyPi IoT Device Management Components](#Step-3-Installation)
-   [Tips](#tips)
-   [Workaround for Manual Test 2](#workarounds)
-   [Next Steps](#NextSteps)

<a id="Introduction"></a>
# Introduction

**About this document**

This document describes the process of setting up a [MyPi Industrial IoT Integrator Board](https://www.embeddedpi.com/) device to connect to an Azure IoT hub. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Install MyPi IoT Edge Device Management Components

<a id="Step-1-Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

- IoT Hub Subscription (Standard) with IoT Edge enabled
- IoT Hub connection string and an IoT Edge Device Configuration String
- SSH client on your desktop computer, such as [PuTTY](http://www.putty.org/), so you can remotely access the command line on the Raspbian system.
-   Required hardware:
	-   [MyPi Industrial IoT Integrator Board](https://www.embeddedpi.com/integrator-board)
	-   8GB or larger MicroSD Card
	-   USB keyboard
	-   USB mouse (optional; you can navigate NOOBS with a keyboard)
	-   USB Mini cable
	-   HDMI cable
	-   TV/ Monitor that supports HDMI
	-   Ethernet cable or Wi-Fi dongle
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

<a id="Step-2-PrepareDevice"></a>
# Step 2: Prepare your Device

-   Install the latest Raspbian operating system on your [MyPi Industrial IoT Integrator Board](https://www.embeddedpi.com/integrator-board) by following the instructions in the [MyPI Install Linux OS guide](http://www.embeddedpi.com/documentation/installing-linux-os/mypi-industrial-raspberry-pi-flashing-the-compute-module).
-   When the installation process is complete, the Raspberry Pi configuration menu (raspi-config) loads. Here you are able to set the time and date for your region and enable a Raspberry Pi camera board, or even create users. Under **Advanced Options**, enable **ssh** so you can access the device remotely with PuTTY or WinSCP. For more information, see <https://www.raspberrypi.org/documentation/remote-access/ssh/>.
-   Connect your Raspberry Pi to your network using an ethernet cable or by using a WiFi dongle on the device.
-   You need to determine the IP address of your Raspberry Pi in order to connect over the network. For more information, see
<https://www.raspberrypi.org/documentation/remote-access/ip-address.md>.
-   Once you see that your board is working, open an SSH terminal program such as [PuTTY](http://www.putty.org/) on your desktop machine.
-   Use the IP address from step 4 as the Host name, Port=22, and Connection type=SSH to complete the connection.
-   When prompted, log in with username **pi**, and password **raspberry**.
-   Create a **root** account using the following command `sudo passwd root` and choosing a new password:

    ![](./media/service-bus-iot-raspberrypi-raspbian-setup/raspbian01.png)

    The root account is necessary in order to install some libraries required by the device SDK.

<a id="Step-3-Installation"></a>
# Step 3: Install the IoT Device Management Components

Download and unpack the MyPi\_IoTHub\_Components.tar.gz archive. Then change to the unpacked MyPi\_IoTHub\_Components directory and run:

    ./install.sh

Then change to /opt/MyPi\_IoTHub\_Components and edit the file config/IOT\_HUB. Assign your device connection string and your IoT Hub connection string to DEVICE\_CONNECTION\_STRING and IOTHUB\_CONNECTION\_STRING.

Finally run:

    systemctl reboot

After reboot, check if the following system services are running:

    systemctl status mypi-iot-component-firmware-update
    systemctl status mypi-iot-component-reboot
    systemctl status mypi-iot-component-update-reboot-time

<a id="tips"/>
# Tips

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application and how to send cloud-to-device messages to the application.


<a id="workarounds"></a>
# Workaround for Manual Test 2

Using MQTT Protocol for the DiretMethod test invocation test, returns error 404103. There seems to be no solution for this problem. A temporary fix seems to be switching to AMQP. This seems to avoid the error 404103 and allows for a successful DirectMethod invocation. 

-   See [GitHub IoT Edge Issue #563][lnk-github-issue-563]. Devs claim the problem should be solved with GA. However, it seems the problem still remains.

<a id="NextSteps"></a>
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
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[lnk-github-issue-563]: https://github.com/Azure/iot-edge-v1/issues/563