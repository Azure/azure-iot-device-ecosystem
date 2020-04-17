---
platform: rtos
device: wib-ek
language: c
---

Getting started with wib evaluation kit + Azure IoT Hub
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare WIB evaluation kit](#PrepareDevice)
-   [Step 3: Build project and run sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"/></a>

# Introduction

WIB series modules provide out-of-the-box Cloud services and analytics to Internet of Things application developers. WIB series modules integrate secure connectivity high performance processor and rich set of peripherals. In the minimum operational configuration WIB modules need to be;

(i) mechanically hosted by an application board,

(ii) properly powered and 

(iii) interfaced with application hardware via peripheral signals.

WIB Evaluation Kit provides the necessary operational configuration for WIB modules and allows for fast and easy application evaluation.

WIB Evaluation Kit allows users to explore the advantages of the WIB series modules while experiencing hassle free interconnection between the module and Inovatink Device Cloud or any other Cloud service.

By following this document you can seamlessly connect WIB to **Azure**, using MQTT and send/receive data to IoT Hub.

<a name="Prerequisites"></a>
# Step 1: Prerequisites

-   [Prepare your development environment][Dev-environment-setup]
-   [Setup your Azure IoT hub][lnk-setup-iot-hub]
-   [Prepare Azure SDK example project][azure-esp-example]

<a name="PrepareDevice"></a>
# Step 2: Prepare wib evaluation kit

WIB Evaluation Kit is a generalized solution that can be used to prototype many different IoT applications. Evaluation board can be used to host all of the WIB series modules ([WIB][lnk-wib], [WIB2G][lnk-wib2g], [WIBNB][lnk-wibnb]). It can be used for both battery and DC adapter powered applications. For more information about the individual module that comes with the kit please refer to module datasheet. 

Evaluation board should be powered by 5V/2A power supply from VIN - GND pins. Board generates 3V3 (200mA) and 5V (200mA) that can be used for peripheral devices.

In order to flash example project to WIB module, connect module programmer to the evaluation board and be sure to check that your OS has installed necessary drivers.

<a name="Build"></a>
# Step 3: Build project and run sample

## Configure project 

-   Enter configuration (menuconfig) and change example configuration with your WiFi SSID & password and Azure connection string that contains Hostname, DeviceID and shared access key in the following format:

    
        HostName=<host_name>;DeviceId=<device_id>;SharedAccessKey=<device_key>

-   Do not forget to initialize submodules (e.g. Azure IoT SDK) in the example project by issuing following command:

        git submodule update --init --recursive

-  Include **custom pinconfig.h** file from [this repository][lnk-ino-getting-started] to your project and use the module peripherals to interact with application.

## Build and flash

- Build your demo and flash it to the module through module programmer.

        make flash

## Results

-   To see the console logs, use the command below:

        make monitor

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
[Dev-environment-setup]: https://docs.espressif.com/projects/esp-idf/en/latest/get-started/
[azure-esp-example]: https://github.com/espressif/esp-azure
[lnk-wib]: http://inovatink.com/statics/pdf/v1.2/Product_Brief_wib_v1.2_EN.pdf
[lnk-wib2g]: http://inovatink.com/statics/pdf/v1.2/Product_Brief_wib2g_v1.2_EN.pdf
[lnk-wibnb]: http://inovatink.com/statics/pdf/v1.2/Product_Brief_wibnb_v1.2_EN.pdf
[lnk-ino-getting-started]: https://github.com/inovatink/wib-ek-getting-started
[lnk-setup-iot-hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-through-portal