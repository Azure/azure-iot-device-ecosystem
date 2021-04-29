| platform      | device | language     |
| :---        |    :----:   |          ---: |
| arduino ide      | arduino nano 33 iot       | c   |


Run a simple C sample on Arduino Nano 33 IoT
===
---

# Table of Contents

- [Introduction](#Introduction)
- [Step 1: Prerequisites](#Step-1-Prerequisites)
- [Step 2: Prepare your Device](#Step-2-PrepareDevice)
- [Step 3: Build and Run the Sample](#Step-3-Build)
- [Next Steps](#NextSteps)

# Introduction
**About this document**
This document describes the process of connecting an Arduino NANO 33 IoT based system to the Azure IoT Hub. This multi-step process includes:
1. Configuring Azure IoT Hub
2. Registering your IoT device
3. Build and deploy Azure IoT SDK on device

# Step 1: Prerequisites
You should have the following items ready before beginning the process:
1. [Arduino NANO 33 IOT](https://store.arduino.cc/usa/nano-33-iot)
2. [Arduino IDE](https://www.arduino.cc/en/Main/Software) (version 1.6.6 or later) installed.
3. [Setup your IoT hub](https://azure.microsoft.com/en-us/free/)
4. [Provision your device and get its credentials](https://portal.azure.com/#home)

# Step 2: Prepare your Device
This section shows you how to set up a development environment for the Azure IoT device SDK with Arduino NANO 33 IoT board.
1. Open Arduino IDE and install the following libraries using the ```Library Manager```, which can be accessed via ```Sketch -> Include Library -> Managed Libraries ...```
    -   WiFiNINA
    -  RTCZero
    - AzureIoTHub
    - AzureIoTProtocol_HTTP
    - AzureIoTProtocol_MQTT
    - AzureIoTUtility
2. Carefully replace "WiFi101.h" to "WiFiNINA.h" in the following files as nano 33 iot board supports WiFiNINA library
    - Arduino/libraries/AzureIoTUtility/src/adapters/sslClient_arduino.cpp
    - Arduino/libraries/AzureIoTUtility/src/samd/NTPClientAz.h
    - Arduino/libraries/AzureIoTUtility/src/samd/sample_init.cpp


# Step 3: Build and Run the sample

1. In the Arduino IDE, select ```Arduino NANO 33 IOT``` as the board in the ```Tools -> Board``` menu.
2. Open the ```iothub_ll_telemetry_sample``` example in the Arduino IDE, ```File -> Examples -> INCOMPATIBLE -> AzureIoTHub -> esp32 -> iothub_ll_telemetry_sample```
3. In ```iot_configs.h```, update the following line with your WiFi accesspoint's SSID:
   ```
   char ssid[] = "yourNetwork"; //  your network SSID (name)
   ```
4. Update the following line with your WiFi accesspoint's password:
   ```
   char pass[] = "secretPassword";    // your network password (use for WPA, or use as key for WEP)
   ```
5. Locate the following code in the ```iot_configs.h```:
   ```
   static const char* connectionString = "[device connection string]";
   ```
6. Replace `[device connection string]` with the device connection string you noted [earlier](#Step-1-Prerequisites). 
7. Add this block of code at the top of ```iothub_ll_telemetry_sample.ino``` :
   ```
   #ifdef is_arduino_board
      #include "Arduino.h"
   #endif
   ```
8. Add this block of code at the end before ```#endif // SAMPLE_INIT_H``` in ```sample_init.h```:

    ```
    #if defined(ARDUINO_ARCH_SAMD)
        #define sample_init m0_sample_init
        #define is_arduino_board
        void m0_sample_init(const char* ssid, const char* password);
   #endif
   ```      
9. Save the changes. Press the ```Verify``` button in the Arduino IDE to build the sample sketch.  

## Deploy the sample

1. Follow the steps from the [build](#Step-3-Build) section.
2. For the Arduino NANO 33 IOT, connect the board to your computer with a USB cable.
3. Select serial port associated with the board in the ```Tools -> Port``` menu.
4. Press the ```Upload``` button in the Arduino IDE to build and upload the sample sketch.

## Run the sample

1. Follow the steps from the [deploy](#deploy) section.
2.   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the **iothub_ll_telemetry_sample** application and how to send cloud-to-device messages to the **iothub_ll_telemetry_sample** application.


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
