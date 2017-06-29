---
platform: secureiot1702
device: cec1702
language: c
---

Run a simple C sample on Microchip SecureIoT1702
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect Microchip SecureIoT1702 board along with Wifi Clicker Board (ATWINC1500) to Microsoft Azure IoT Hub, by using the X.509 based MQTT application sample, provided with Azure IoT Device SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

## 1.1 Development Environment

-   **Keil µVision IDE** - For project build, downloading and debugging code
-   **SPI Flash Programmer** – For loading spi image in flash
-   **ComXDBG.exe** (provided with package) or any other serial terminal installed in your PC – for viewing trace messages from SecureIoT1702 board
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   BIOS PS306 device.

## 1.2 Hardware components

-   SecureIoT1702 board
-   ATWINC1510 Clicker Board

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Attach the ATWINC1510 Clicker board on mikroBUS slot 1 on SecureIoT1702 board as shown in the figure.

    ![board](media/microchip-secureiot1702/board.jpg)

<a name="Build"></a>
# Step 3: Build and Run the sample

##  3.1 Build SDK and sample code

1.  Download the [SecureIoT1702_Azure_IoT_build_0100.zip](http://ww1.microchip.com/downloads/en/softwarelibrary/cec1702_azure_iot/secureiot1702_azure_iot_build_0100.zip) package. This package contains all the required drivers to use with the SecureIoT1702 board with Winc1500 clicker board, together with pre-integrated Microsoft Azure IoT C SDK.
The project uses mbedTLS as the TLS stack, which has been added to the project as a library:

    `SecureIoT1702_Azure_IoT\framework\mbedTLS\libmbedtls240.lib`

2.  Unzip the package and open the Keil project file

    `SecureIoT1702_Azure_IoT\demo_projects\SecureIoT1702\azure_iot_cec1702.uvprojx`

3.  The MQTT application is **iothub_client_sample_x509**. It uses MQTT as the transport for communicating to the AZURE IOT hub.

    The certificates and keys required for authentication is hardcoded in the C file (PEM format).

    -   static const char* rootCA = “  “
    -   static const char* x509certificate = “ “ 
    -   static const char* x509privatekey = “ “

    `SecureIoT1702_Azure_IoT\demo_projects\SecureIoT1702\src\apps\iothub_client_sample_x509.c`

The rootCA for Azure IoT – Baltimore root CA is pre-programmed in iothub_client_sample_x509. 

Update the x509certificate and x509privatekey to match your X.509 credentials.

Please refer [Secure your IoT deployment](https://github.com/Microsoft/azure-docs/blob/master/includes/iot-secure-your-deployment.md) and [Use IoT Hub security tokens and X.509](https://github.com/Azure/azure-content-nlnl/blob/master/articles/iot-hub/iot-hub-sas-tokens.md) certificates for more details on X.509 security.

The IoT Hub connection string is present in the connectionString variable; update this value to match your connection string.

4.  Wifi Configuration - Currently the code is configured to connect through WPA-PSK. The SSID and password are set statically in **winc1500_connect.c**

    `SecureIoT1702_Azure_IoT\demo_projects\SecureIoT1702\src\platform\winc\ winc1500_connect.c`

    ```
    #define CONN_SSID						"TP-LINK_6934"
    #define CONN_PSK_PWD					"03893708"
    ```

    Modify the above values to match your wifi router. 

5.  Build Output

    ![build](media/microchip-secureiot1702/build-output.png)
 
    Compiler: Keil uVision V5.20.0.0

    The spi_image (spi_image.bin) is created as part of the post-build process. The output files are placed in: `SecureIoT1702_Azure_IoT\demo_projects\SecureIoT1702\targets`

## 3.2 Build Firmware into Flash

The CEC1702 firmware application is stored in the external SPI flash device. 

A programmer header is added for using an external SPI flash programmer (such as Dediprog SF100) to program the firmware application.

## 3.3 Connect and send messages to Azure IoT Hub

To view the UART traces:

1.  Connect USB cable between SecureIoT1702 board and Windows host 
2.  After driver installation, start ComXDBG.exe 
SecureIoT1702_Azure_IoT\utilities\ ComEDBG \ ComXDBG.exe
3.  Select FTDIBUS COM port 
    For example: for the below options we would enter 0

    ![connect](media/microchip-secureiot1702/connect.png)

4.  You should be able to view UART traces from the SecureIoT1702 board.

    Alternatively, you can use your serial terminal (e.g. TeraTerm for Windows) with the following parameters:

    -   BaudRate : 115200
    -   Data : 8-bit
    -   Parity : None
    -   Stop : 1 bit
    -   Flow Control : None

    **Sample log**

    ```
    [13:28:11.006] ORIGINAL IMAGE
    [13:28:11.006] AZURE IOT Node
    [13:28:11.006] Microchip CEC1702
    [13:28:11.006]  Ver 1.0.1
    [13:28:11.006] Apr 13 2017 13:27:29
    [13:28:11.312] $Ts:
    [13:28:11.813] winc1500_wifi_cb: 2c
    [13:28:11.813] M2M_WIFI_RESP_CON_STATE_CHANGED: CONNECTED
    [13:28:11.813] winc1500_wifi_cb: 32
    [13:28:11.875] M2M_WIFI_REQ_DHCP_CONF: IP is 192.168.0.100
    [13:28:11.875] WINC is connected to TP-LINK_258C successfully!
    [13:28:11.875] winc1500_wifi_init: Done
    [13:28:11.875] Initializing rando.
    [13:28:11.875] Starting the IoTHub client sample x509...
    [13:28:11.875] Info: IoT Hub SDK for C, version 1.1.8
    [13:28:11.875] socketio_create:
    [13:28:12.254] IoTHubClient_LL_SetMessageCallback...successful.
    [13:28:12.254] IoTHubClient_LL_SendEventAsync accepted message [0] for transmission to IoT Hub.
    [13:28:12.254] socketio_open:
    [13:28:12.309] winc1500_wifi_cb: 20
    [13:28:12.309] SERVER IP is 40.76.71.185
    [13:28:12.309] mbedtls_connect:
    [13:28:20.804] ->  CONNECT | VER: 4 | KEEPALIVE: 240 | FLAGS: 128 | USERNAME: azure-iothub-mchp-ny-1.azure-devices.net/my-device-1/api-version=2016-11-14&Device
    ClientType=iothubclient%2F1.1.8 | CLEAN: 0
    [13:28:20.804] mbedtls_connect: sts 0
    [13:28:21.764] <-  CONNACK | SESSION_PRESENT: true | RETURN_CODE: 0x0
    [13:28:23.773] IoTHubClient_LL_SendEventAsync accepted message [1] for transmission to IoT Hub.
    [13:28:23.830] ->  SUBSCRIBE | PACKET_ID: 2 | TOPIC_NAME: devices/my-device-1/messages/devicebound/# | QOS: 1
    [13:28:24.838] <-  SUBACK | PACKET_ID: 2 | RETURN_CODE: 1
    [13:28:26.844] IoTHubClient_LL_SendEventAsync accepted message [2] for transmission to IoT Hub.
    [13:28:28.853] IoTHubClient_LL_SendEventAsync accepted message [3] for transmission to IoT Hub.
    [13:28:28.958] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/my-device-1/messages/events/PropName=PropMsg_0 | PACKET_ID: 3 | PAYLOAD_LEN: 48
    [13:28:29.010] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/my-device-1/messages/events/PropName=PropMsg_1 | PACKET_ID: 4 | PAYLOAD_LEN: 48
    [13:28:29.063] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/my-device-1/messages/events/PropName=PropMsg_2 | PACKET_ID: 5 | PAYLOAD_LEN: 48
    [13:28:29.118] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/my-device-1/messages/events/PropName=PropMsg_3 | PACKET_ID: 6 | PAYLOAD_LEN: 48
    [13:28:30.176] <-  PUBACK | PACKET_ID: 3
    [13:28:30.176] Confirmation[0] received for message tracking id = 0 with result= IOTHUB_CLIENT_CONFIRMATION_OK
    [13:28:30.176] <-  PUBACK | PACKET_ID: 4
    [13:28:30.176] Confirmation[1] received for message tracking id = 1 with result= IOTHUB_CLIENT_CONFIRMATION_OK
    [13:28:30.176] <-  PUBACK | PACKET_ID: 5
    [13:28:30.176] Confirmation[2] received for message tracking id = 2 with result= IOTHUB_CLIENT_CONFIRMATION_OK
    [13:28:30.176] <-  PUBACK | PACKET_ID: 6
    [13:28:30.176] Confirmation[3] received for message tracking id = 3 with result= IOTHUB_CLIENT_CONFIRMATION_OK
    [13:28:32.193] IoTHubClient_LL_SendEventAsync accepted message [4] for transmission to IoT Hub.
    [13:28:32.198] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/my-device-1/messages/events/PropName=PropMsg_4 | PACKET_ID: 7 | PAYLOAD_LEN: 48
    [13:28:33.264] <-  PUBACK | PACKET_ID: 7
    [13:28:33.264] Confirmation[4] received for message tracking id = 4 with result= IOTHUB_CLIENT_CONFIRMATION_OK	
    ```

    See [Device Explorer](https://github.com/fsautomata/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to learn how to observe the messages IoT Hub receives from the device.

## 3.4 Receive messages to Azure IoT Hub 

    See [Device Explorer](https://github.com/fsautomata/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to learn how to send cloud-to-device messages from IoT hub. The received messages are displayed in the serial terminal. 

    Example:

    ```
    [16:34:10.237] <-  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE = 0x01 | TOPIC_NAME: devices/my-device-1/messages/devicebound/%24.mid=84c08717-2be1-404c-9e5d-62f4565ca58b&%24.to=%2Fdevices%2Fmy-device-1%2Fmessages%2FdeviceBound&iothub-ack=full | PACKET_ID: 36 | PAYLOAD_LEN: 47
    [16:34:10.237] Received Message [0]
    [16:34:10.237]  Message ID: 84c08717-2be1-404c-9e5d-62f4565ca58b
    [16:34:10.237]  Correlation ID: <null>
    [16:34:10.237]  Data: <<<6/26/2017 4:34:07 PM - My command to the device>>> & Size=47

    [16:35:13.669] <-  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE = 0x01 | TOPIC_NAME: devices/my-device-1/messages/devicebound/%24.mid=aaefb161-9f06-4d34-8c72-90a10cbebabe&%24.to=%2Fdevices%2Fmy-device-1%2Fmessages%2FdeviceBound&iothub-ack=full&temperature=45&windSpeed=30 | PACKET_ID: 37 | PAYLOAD_LEN: 47
    [16:35:13.669] Received Message [1]
    [16:35:13.669]  Message ID: aaefb161-9f06-4d34-8c72-90a10cbebabe
    [16:35:13.669]  Correlation ID: <null>
    [16:35:13.669]  Data: <<<6/26/2017 4:35:10 PM - My command to the device>>> & Size=47
    [16:35:13.669]  Message Properties:
    [16:35:13.669]  Key: temperature Value: 45
    [16:35:13.669]  Key: windSpeed Value: 30
    ```

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
[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

