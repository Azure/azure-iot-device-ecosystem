---
platform: mbed
device: SadeIoTCloudGateway
language: c
---

Run a simple C sample on SadeIoTCloudGateway device running mbed
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Step-1-Prerequisites)
-   [Step 2: Prepare your Device](#Step-2-PrepareDevice)
-   [Step 3: Build and Run the Sample](#Step-3-Build)
-   [Tips](#tips)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes the steps for connecting an [mbed-SADEIotCloudGateway](<http://developer.mbed.org/platforms/IBMEthernetKit>) device to Azure IoT Hub. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Step-1-Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:
-   Computer with Git client installed and access to the
    [azure-iot-sdk-c](https://github.com/Azure/azure-iot-sdk-c) GitHub public repository.
-   SSH client on your desktop computer, such as [PuTTY](http://www.putty.org/), so you can remotely access the command line on the SadeIOTCloudGateway.
-   Required hardware: [mbed-SADEIotCloudGateway](<http://developer.mbed.org/platforms/IBMEthernetKit>), SADETemperatureExtensionBoard
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

<a name="Step-2-PrepareDevice"></a>
# Step 2: Prepare your Device

-   Connect the board to your network using an Ethernet cable. This step is required, as the sample depends on Internet access.

-   Plug the device into your computer using a micro-USB cable. Be sure to attach the cable to the USB port next to the reset button on the SADEIoTCloudGateway device.

-   Follow the [instructions on the mbed handbook](https://developer.mbed.org/handbook/SerialPC) to set up the serial connection with your device from your development machine. If you are on Windows, install the Windows serial port drivers located [here](http://developer.mbed.org/handbook/Windows-serial-configuration#1-download-the-mbed-windows-serial-port).

<a name="Step-3-Build"></a>
# Step 3: Build and Run the sample

## Create mbed project and import the sample code

-   In your web browser, go to the mbed.org [developer site](https://developer.mbed.org/). If you haven't signed up, you will see an option to create a new account (it's free). Otherwise, log in with your account credentials. Then click on **Compiler** in the upper right-hand corner of the page. This should bring you to the Workspace Management interface.

-   Make sure the hardware platform you're using appears in the upper right-hand corner of the window, or click the icon in the right-hand corner to select your hardware platform.

-   Click **Import** on the main menu. Then click the **Click here** to import from URL link next to the mbed globe logo.

	![][1]

-   In the popup window, enter the link for the sample code https://developer.mbed.org/users/AzureIoTClient/code/iothub_client_sample_amqp/ (note that if you want to try the sample using HTTP instead of AMQP, you can import this other sample: https://developer.mbed.org/users/AzureIoTClient/code/iothub_client_sample_http/

	![][2]

-   You can see in the mbed compiler that importing this project imported various libraries. Some are provided and maintained by the Azure IoT team ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [iothub_http_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_http_transport/), [azure-uamqp-c](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp_c/)), while others are third party libraries available in the mbed libraries catalog.

	![][3]

-   Open iothub_client_sample_amqp/iothub_client_sample_amqp.c, and replace "[device connection string]" with the device connection string you noted [earlier](#Step-1-Prerequisites):

	![][4]

-   Save the changes.

## Build and run the program

-   Click **Compile** to build the program. You can safely ignore any warnings, but if the build generates errors, fix them before proceeding.

	![][5]

-   If the build is successful, a .bin file with the name of your project is generated. Copy the .bin file to the device. Saving the .bin file to the device causes the current terminal session to the device to reset. When it reconnects, reset the terminal again manually, or start a new terminal. This enables the mbed device to reset and start executing the program.

-   Connect to the device using an SSH terminal program, such as PuTTY. You can determine which serial port your device uses by checking the Windows Device Manager:

	![][6]

-   In PuTTY, click the **Serial** connection type. The device most likely connects at 115200, so enter that value in the **Speed** box. Then click **Open**:

	![][7]

The program starts executing. You may have to reset the board (press CTRL+Break or press on the board's reset button) if the program does not start automatically when you connect.

## Monitor device data and send messages

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application and how to send cloud-to-device messages to the application.


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
[1]: ./media/mbed1.png
[2]: ./media/mbed2.png
[3]: ./media/mbed3.png
[4]: ./media/mbed4.png
[5]: ./media/mbed5.png
[6]: ./media/mbed6.png
[7]: ./media/mbed7.png

[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
