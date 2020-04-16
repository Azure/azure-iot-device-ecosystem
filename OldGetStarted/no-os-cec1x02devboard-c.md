---
platform: no os
device: cec1x02devboard
language: c
---

Run a simple C sample on CEC1x02DevBoard device running No OS
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

This document describes how to connect CEC1x02DevBoard device running No OS with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

### 1.1	Development Environment

-   ***Keil µVision IDE*** - For project build, downloading and debugging code
-   ***mikroProg Suite For ARM*** – For loading spi image in flash
-   ***ComXDBG.exe*** (provided with package) or any other serial terminal installed in your PC – for viewing trace messages from devBoard1x02 board
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

### 1.2 Hardware components

-   CEC1x02 devBoard board
-   ATWINC1510 Clicker Board

# Step 2: Prepare your Device

Attach the ATWINC1510 Clicker board on mikroBUS slot 1 on CEC1702 devBoard board as shown in the figure

![](./media/no-os-cec1x02devboard-c/1.jpg)

<a name="Build"></a>
# Step 3: Build and Run the sample

### 3.1 Build SDK and sample

-   Download the [devBoard\_Azure\_IoT\_build](http://www.microchip.com/SWLibraryWeb/product.aspx?product=CEC1702_AZURE_DICE) package. This package contains all the required drivers to use with the CEC1x02 devBoard board with Winc1500 clicker board, together with pre-integrated Microsoft Azure IoT C SDK.
The project uses mbedTLS as the TLS stack, which has been added to the project as a library:

    `devBoard\_Azure\_IoT\Src\APP\framework\mbedTLS\libmbedtls240.lib`

-   Unzip the package and open the Keil project file

    `devBoard\_Azure\_IoT\MDK-ARM\riotDemo.uvprojx`

-   The MQTT application is x509_client_sample. It uses MQTT as the transport for communicating to the AZURE IOT hub.

    `devBoard\_Azure\_IoT\Src\APP\apps\x509\_client_sample.c`

    The certificates and keys required for authentication is generated through DICE (Device Identification Composition Engine).

    For testing purpose the UDS (Unique Device Secret) for the device, uses a test value hardcorded in dps\_hsm\_riot.c

        static unsigned char g\_uds\_seed[DICE\_UDS\_LENGTH] = { 
            0x54, 0x10, 0x5D, 0x2E, 0xCD, 0x07, 0xF9, 0x01,    
            0x99, 0xB3, 0x95, 0xC7, 0x42, 0x61, 0xA0, 0x8C, 
            0xFF, 0x27, 0x1A, 0x0D, 0xF6, 0x6F, 0x1F, 0xE0,
            0x00, 0x34, 0xBB, 0x11, 0xF7, 0x98, 0x9A, 0x12 };

    `devBoard\_Azure\_IoT\Src\APP\framework\azure\dps\_client\adapters\dps\_hsm\_riot.c`
	
    The thumbprint for the generated X.509 certificate using the test UDS is 69CF2777F616BA27926E8BA7D947B8E4622841A4  

    Later when you want to use the UDS value from efuse, you can uncomment the macro ***USE\_TEST\_UDS*** in ***dps\_hsm\_riot.c***

    Please refer [Secure your IoT deployment](https://github.com/Microsoft/azure-docs/blob/master/includes/iot-secure-your-deployment.md) and [Control Access to IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-security#supported-x509-certificates) for more details on X.509 security.

    In our case, the X.509 certificate is generated for the device through DICE and RIoT process. See [Device Identity with DICE and RIoT: Keys and Certificates](https://www.microsoft.com/en-us/research/publication/device-identity-dice-riot-keys-certificates/) for more details.

    The DICE, RIoT and Azure application code are built together as one firmware image; they are organized separately in memory by using linker options. See riotDemo.sct for the linker file memory layouts.

    `devBoard\_Azure\_IoT\MDK-ARM\riotDemo.sct`

-   Update your iothub host url and device id in x509_client_sample.c

        #define IOTHUB_CONNECTION_STRING    "azure-iothub-mchp-1.azure-devices.net"
        #define IOTHUB_DEVICE_ID            	   "dice-device-1"

-   Wifi Configuration - Currently the code is configured to connect through WPA-PSK. The SSID and password are set statically in ***winc1500\_connect.c***

    `devBoard_Azure_IoT\Src\APP\platform\winc\winc1500_connect.c`

        define CONN_SSID          "TP-LINK_6934"
        define CONN_PSK_PWD          "03893708"

    Modify the above values to match your wifi router.

-   Build Output

![](./media/no-os-cec1x02devboard-c/2.jpg)

Compiler: Keil uVision V5.23.0.0

The spi\_image (`spi\_image.bin`) is created as part of the post-build process. The output files are placed in: `devBoard\_Azure\_IoT\MDK-ARM\targets`

### 3.2 Build Firmware into Flash

-   The CEC1x02 firmware application is stored in the external SPI flash device.
-   Use the mikroProg Suite For ARM to program the SPI in the CEC1x02 PIM.

### 3.3 Connect and send messages to Azure IoT Hub

To view the UART traces:

-   connect USB cable between devBoard board and Windows host 
-   After driver installation, start ComXDBG.exe

        devBoard\_Azure\_IoT\utilities\ ComEDBG \ ComXDBG.exe
-   Select FTDIBUS COM port

    For example; for the below options we would enter 0

![](./media/no-os-cec1x02devboard-c/3.png)

-   You should be able to view UART traces from the devBoard board.

    Alternatively, you can use your serial terminal (e.g. TeraTerm for Windows) with the following parameters:

    -   BaudRate : 115200
    -   Data : 8-bit
    -   Parity : None
    -   Stop : 1 bit
    -   Flow Control : None

**Sample log**

```
[20:39:04.932] AZURE IoT Node
[20:39:04.932] Microchip CEC1702 devBoard 01
[20:39:04.932] Firmware Version : CEC1702_devBoard_diceRIoT_build_0800
[20:39:04.932] Dec 28 2017 20:25:54
[20:39:04.932] ------------------------------------------------
[20:39:05.882] winc1500_wifi_cb: 2c
[20:39:05.882] M2M_WIFI_RESP_CON_STATE_CHANGED: CONNECTED
[20:39:05.882] winc1500_wifi_cb: 32
[20:39:05.882] M2M_WIFI_REQ_DHCP_CONF: IP is 192.168.43.70
[20:39:05.882] WINC is connected to Akshaya successfully!
[20:39:05.882] winc1500_wifi_init: Done
[20:39:05.882] Initializing rando.
[20:39:05.882] DPS_HSM: initialize_riot_system:
[20:39:05.933] Iothub Version: 1.1.22
[20:39:05.933] -------------------- IoTHubClient_LL_CreateFromDeviceAuth -------
[20:39:05.933] IOTHUB CLIENT: uri azure-iothub-mchp-ny-1.azure-devices.net
[20:39:05.933] IOTHUB CLIENT: device id dice-device-1
[20:39:05.933] DPS_HSM: dps_hsm_riot_create:
[20:39:05.933] RIOT: CDI
[20:39:05.933] ce1c95020a4902a87fde4dbf8977b68362a944eac301c4c6c25828c92bd80780
[20:39:07.039] RIOT: Device Certificate
[20:39:07.089] -----BEGIN CERTIFICATE-----
[20:39:07.089] MIIBdTCCARugAwIBAgIFDg0MCwowCgYIKoZIzj0EAwIwNDESMBAGA1UEAwwJcmlvdC1yb290MQswCQYDVQQGDAJVUzERMA8GA1UECgwITVNSX1RFU1QwHhcNMTcwMTAxMDAwMDAwWhcNMzcwMTAxMDAwMDAwWjA7MRkwFwYDVQQDDBByaW90LXNpZ25lci1jb3JlMQswCQYDVQQGDAJVUzERMA8GA1UECgwITVNSX1RFU1QwWTATBgcqhkjOPQIBBggqhkjOPQMBBwNCAAR2C/Fa9fotRGPpEN91dRjfT7iHPyBnpx/blWJ9CFvfA+65lPGvT2cuyVRh0XjaoDpWwhSPV/KbqYu/YrpqAu+9oxMwETAPBgNVHRMECDAGAQH/AgEBMAoGCCqGSM49BAMCA0gAMEUCIQDFhqlOoamzbIz/1NUWm2njxwjDcxM/uSOw8fc2iUkEEwIgBl2wjtTF2TXnWfN8wTj/vHRysR3lK6mit4J
jvO9C0oE=
[20:39:07.089] -----END CERTIFICATE-----
[20:39:07.390] DPS_HSM: dps_hsm_riot_get_certificate:
[20:39:07.390] RIOT: Alias Certificate
[20:39:07.390] -----BEGIN CERTIFICATE-----
[20:39:07.490] MIICNjCCAdugAwIBAgIFCgsMDQ4wCgYIKoZIzj0EAwIwOzEZMBcGA1UEAwwQcmlvdC1zaWduZXItY29yZTELMAkGA1UEBgwCVVMxETAPBgNVBAoMCE1TUl9URVNUMB4XDTE3MDEwMTAwMDAwMFoXDTM3MDEwMTAwMDAwMFowOzEZMBcGA1UEAwwQcmlvdC1kZXZpY2UtY2VydDELMAkGA1UEBgwCVVMxETAPBgNVBAoMCE1TUl9URVNUMFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEXl8k/JM5Trrd+T2hCzH8J8FzdMo+nqEBctNE0wUQWesM+QbsF1PBmEHxb/FpqegLlleJW4niMxaN8z3J7T/imKOByzCByDAWBgNVHSUBAf8EDDAKBggrBgEFBQcDAjCBrQYDVR0RAQH/BIGiMIGfoIGcBgorBgEEAYI3WQMBMIGNAgEBMFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgA
EdgvxWvX6LURj6RDfdXUY30+4hz8gZ6cf25VifQhb3wPuuZTxr09nLslUYdF42qA6VsIUj1fym6mLv2K6agLvvTAtBglghkgBZQMEAgEEIGvpsYTJN8KOEi7uUSto6o4Aw90VnqToXoTLqWb0Rs1OMAoGCCqGSM49BAMCA0kAMEYCIQDFhqlOoamzbIz/1NUWm2njxwjDcxM/uSOw8fc2iUkEEwIhAMts/WpPgP8xjvYkDaI4n0x30euQUj78oTvlY4Qp61yk
[20:39:07.490] -----END CERTIFICATE-----
[20:39:07.490] DPS_HSM: dps_hsm_riot_get_alias_key:
[20:39:07.490] RIOT: Alias key
[20:39:07.490] -----BEGIN EC PRIVATE KEY-----
[20:39:07.490] MHcCAQEEINXd3WSS7LqDEFGdpbQi0V51w01XkpMVPUYlG9V7/ldUoAoGCCqGSM49AwEHoUQDQgAEXl8k/JM5Trrd+T2hCzH8J8FzdMo+nqEBctNE0wUQWesM+QbsF1PBmEHxb/FpqegLlleJW4niMxaN8z3J7T/imA==
[20:39:07.490] -----END EC PRIVATE KEY-----
[20:39:07.490] DPS_HSM: dps_hsm_riot_destroy:
[20:39:07.490] socketio_create:
[20:39:07.490] TLSIO MBEDTLS: setoption: x509EccCertificate
[20:39:07.542] TLSIO MBEDTLS: setoption: x509EccAliasKey
[20:39:07.542] TLSIO MBEDTLS: setoption: TrustedCerts
[20:39:07.542] Sending temperature reading to IoTHub every 2 seconds
[20:39:07.542] socketio_open:
[20:39:07.592] winc1500_wifi_cb: 20
[20:39:07.642] winc1500_wifi_cb: 1b
[20:39:07.842] SERVER IP is 40.76.71.185
[20:39:08.092] winc1500_connect: socket: 0
[20:39:14.595] mbedtls_connect: sts 0
[20:39:14.897] ->  CONNECT | VER: 4 | KEEPALIVE: 25 | FLAGS: 128 | USERNAME: azure-iothub-mchp-ny-1.azure-devices.net/dice-device-1/api-version=2016-11-14&DeviceClientType=iothubclient%2f1.1.22%20(CEC1702) | CLEAN: 0
[20:39:15.897] <-  CONNACK | SESSION_PRESENT: true | RETURN_CODE: 0x0
[20:39:17.903] IoTHubClient_LL_SendEventAsync accepted message [1] for transmission to IoT Hub.
[20:39:18.203] ->  SUBSCRIBE | PACKET_ID: 2 | TOPIC_NAME: devices/dice-device-1/messages/devicebound/# | QOS: 1
[20:39:19.204] <-  SUBACK | PACKET_ID: 2 | RETURN_CODE: 1
[20:39:21.255] IoTHubClient_LL_SendEventAsync accepted message [2] for transmission to IoT Hub.
[20:39:24.608] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 3 | PAYLOAD_LEN: 83
[20:39:26.509] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 4 | PAYLOAD_LEN: 83
[20:39:27.510] <-  PUBACK | PACKET_ID: 3
[20:39:27.510] <-  PUBACK | PACKET_ID: 4
[20:39:29.517] IoTHubClient_LL_SendEventAsync accepted message [3] for transmission to IoT Hub.
[20:39:29.868] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 5 | PAYLOAD_LEN: 83
[20:39:30.869] <-  PUBACK | PACKET_ID: 5
[20:39:32.871] IoTHubClient_LL_SendEventAsync accepted message [4] for transmission to IoT Hub.
[20:39:33.172] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 6 | PAYLOAD_LEN: 83
[20:39:34.175] <-  PUBACK | PACKET_ID: 6
[20:39:36.175] IoTHubClient_LL_SendEventAsync accepted message [5] for transmission to IoT Hub.
[20:39:36.477] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 7 | PAYLOAD_LEN: 83
[20:39:37.477] <-  PUBACK | PACKET_ID: 7
[20:39:39.480] IoTHubClient_LL_SendEventAsync accepted message [6] for transmission to IoT Hub.
[20:39:39.831] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 8 | PAYLOAD_LEN: 83
[20:39:40.832] <-  PUBACK | PACKET_ID: 8
[20:39:42.883] IoTHubClient_LL_SendEventAsync accepted message [7] for transmission to IoT Hub.
[20:39:43.183] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 9 | PAYLOAD_LEN: 83
[20:39:44.184] <-  PUBACK | PACKET_ID: 9
[20:39:46.185] IoTHubClient_LL_SendEventAsync accepted message [8] for transmission to IoT Hub.
[20:39:46.586] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 10 | PAYLOAD_LEN: 83
[20:39:47.588] <-  PUBACK | PACKET_ID: 10
[20:39:49.590] IoTHubClient_LL_SendEventAsync accepted message [9] for transmission to IoT Hub.
[20:39:49.890] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 11 | PAYLOAD_LEN: 83
[20:39:50.890] <-  PUBACK | PACKET_ID: 11
[20:39:52.944] IoTHubClient_LL_SendEventAsync accepted message [10] for transmission to IoT Hub.
[20:39:53.395] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 12 | PAYLOAD_LEN: 83
[20:39:54.396] <-  PUBACK | PACKET_ID: 12
[20:39:56.403] IoTHubClient_LL_SendEventAsync accepted message [11] for transmission to IoT Hub.
[20:39:56.754] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 13 | PAYLOAD_LEN: 83
[20:39:57.755] <-  PUBACK | PACKET_ID: 13
[20:39:59.758] IoTHubClient_LL_SendEventAsync accepted message [12] for transmission to IoT Hub.
[20:40:00.160] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 14 | PAYLOAD_LEN: 83
[20:40:01.161] <-  PUBACK | PACKET_ID: 14
[20:40:03.162] IoTHubClient_LL_SendEventAsync accepted message [13] for transmission to IoT Hub.
[20:40:03.512] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 15 | PAYLOAD_LEN: 83
[20:40:04.514] <-  PUBACK | PACKET_ID: 15
[20:40:06.565] IoTHubClient_LL_SendEventAsync accepted message [14] for transmission to IoT Hub.
[20:40:06.868] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 16 | PAYLOAD_LEN: 83
[20:40:07.871] <-  PUBACK | PACKET_ID: 16
[20:40:09.873] IoTHubClient_LL_SendEventAsync accepted message [15] for transmission to IoT Hub.
[20:40:10.225] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 17 | PAYLOAD_LEN: 83
[20:40:11.226] <-  PUBACK | PACKET_ID: 17
[20:40:13.229] IoTHubClient_LL_SendEventAsync accepted message [16] for transmission to IoT Hub.
[20:40:13.580] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 18 | PAYLOAD_LEN: 83
[20:40:14.584] <-  PUBACK | PACKET_ID: 18
[20:40:16.592] IoTHubClient_LL_SendEventAsync accepted message [17] for transmission to IoT Hub.
[20:40:16.893] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 19 | PAYLOAD_LEN: 83
[20:40:17.943] <-  PUBACK | PACKET_ID: 19

```

See [Device Explorer](https://github.com/fsautomata/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to learn how to observe the messages IoT Hub receives from the device.

### 3.4 Receive messages to Azure IoT Hub

See [Device Explorer](https://github.com/fsautomata/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to learn how to send cloud-to-device messages from IoT hub. The received messages are displayed in the serial terminal.

**Example:**

```
[20:44:03.366] <-  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE = 0x01 | TOPIC_NAME: devices/dice-device-1/messages/devicebound/%24.mid=22a4184a-ecbb-473b-b816-3d5fae0a63e5&%24.to=%2Fdevices%2Fdice-device-1%2Fmessages%2FdeviceBound&iothub-ack=full&led=green | PACKET_ID: 3 | PAYLOAD_LEN: 0
[20:44:03.366] Received Message [0]
[20:44:03.366]  Message ID: 22a4184a-ecbb-473b-b816-3d5fae0a63e5
[20:44:03.366]  Correlation ID: <null>
[20:44:03.377]  Data: <<<>>> & Size=0
[20:44:03.377]  Message Properties:
[20:44:03.377]  Key: led Value: green
[20:44:05.754] IoTHubClient_LL_SendEventAsync accepted message [77] for transmission to IoT Hub.
[20:44:06.106] ->  PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/dice-device-1/messages/events/ | PACKET_ID: 79 | PAYLOAD_LEN: 83
[20:44:07.107] <-  PUBACK | PACKET_ID: 79

```
### 3.5 Steps to use UDS data from eFuse

1.   Disable (comment) the macro USE_TEST_UDS in dps_hsm_riot.c
2.   Compile the code and generate spi image. 
3.   Program the spi_image.bin (See step 3.2)

    **Note:** Once you program this image to SPI; the UDS will be locked after the bootrom loads and executes this application. Hence later if you run any application through JTAG, it will always read the UDS as 0.

### Deriving the X.509 certificate and the thumbprint

-   On powering the board, the alias certificate is displayed on the serial terminal, after the following the text:
RIOT: Alias Certificate

    **Example:**

```
-----BEGIN CERTIFICATE-----
[15:11:14.359] MIIBdTCCARugAwIBAgIFDg0MCwowCgYIKoZIzj0EAwIwNDESMBAGA1UEAwwJcmlvdC1yb290MQswCQYDVQQGDAJVUzERMA8GA1UECgwITVNSX1RFU1QwHhcNMTcwMTAxMDAwMDAwWhcNMzcwMTAxMDAwMDAwWjA7MRkwFwYDVQQDDBByaW90LXNpZ25lci1jb3JlMQswCQYDVQQGDAJVUzERMA8GA1UECgwITVNSX1RFU1QwWTATBgcqhkjOPQIBBggqhkjOPQMBBwNCAAR2C/Fa9fotRGPpEN91dRjfT7iHPyBnpx/blWJ9CFvfA+65lPGvT2cuyVRh0XjaoDpWwhSPV/KbqYu/YrpqAu+9oxMwETAPBgNVHRMECDAGAQH/AgEBMAoGCCqGSM49BAMCA0gAMEUCIQDFhqlOoamzbIz/1NUWm2njxwjDcxM/uSOw8fc2iUkEEwIgBl2wjtTF2TXnWfN8wTj/vHRysR3lK6mit4J
jvO9C0oE=
[15:11:14.359] -----END CERTIFICATE-----

```

-   Copy the text from ‘BEGIN CERTIFICATE’ to ‘END CERTIFICATE’ to a text editor file. 
Remove the time stamps, and save with .crt extension. 

-   Double click the file with .crt extension; in details tab, thumbprint bytes can be found. 

![](./media/no-os-cec1x02devboard-c/4.jpg)

-   The thumbprint should be used as primary key when registering the device in IoT hub. 
Use upper case for the thumbprint bytes.

![](./media/no-os-cec1x02devboard-c/5.jpg)

Once the thumbprint is registered for the device; on the next power-on, the device should be able to connect to the Azure IoT hub.

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


























   
