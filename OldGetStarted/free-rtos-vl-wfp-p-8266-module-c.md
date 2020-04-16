---
platform: free-rtos
device: vl-wfp-p-8266 module
language: c
---

# Table of Contents
-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Step-1-Prerequisites)
-   [Step 2: Build and Run the Sample](#Step-2-Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

The following document describes the process of connecting VL-WFP-P-8266 (the chip is esp8266) module to Azure IoT Hub.

<a name="Step-1-Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   Ubuntu x86 machine (for cross-compiling)
-   Vane VL-WFP-P-8266 module equipment
-   A computer with 2 serial port lines (a serial port print output for a connection device, another for Flash Loader Demonstrator)
-   The wireless router can wireless internet.
-   Open the flash download tool **ESP\_DOWNLOAD\_TOOL\_V2.4.exe**

<a name="Step-2-Build"></a>
# Step 2: Build and Run the sample

This section shows you how to Compile library file of Azure IoT.

1.  Download **esp8266\_rtos\_sdk** to **Ubuntu x86 machine** from platform of vane

2.  Download **azure-iot-sdk-c** from <https://github.com/Azure/azure-iot-sdk-c>

3.  Download **azure-c-shared-utility** from <https://github.com/Azure/azure-c-shared-utility>

4.  Download **azure-umqtt-c** from <https://github.com/Azure/azure-umqtt-c>

##3. Build the sample

1.  Create following folders:

        cd esp8266_rtos_sdk/third_partty
        mkdir iothub_client azure_c_shared_utility azure_umqtt_c

        cd esp8266_rtos_sdk/include
        mkdir iothub_client azure_c_shared_utility azure_umqtt_c

2.  Copy following files from **azure-iot-sdk-c** to **esp8266\_rtos\_sdk/third\_partty/iothub\_client**:

        Iothub_client.c
        iothub_client_authorization.c
        iothub_client_diagnostic.c
        iothub_client_ll.c
        Iothub_client_retry_control.c
        iothub_client_sample_mqtt_esp8266.c
        iothub_message.c
        Iothubtransport.c
        iothubtransport_mqtt_common.c
        iothubtransportmqtt.c

    and build  own makefile (makefile file can refer to the makefile of the Library under the third_partty).

3.  Copy following files from **azure-iot-sdk-c** to **esp8266\_rtos\_sdk/include/iothub\_client**:

        Iothub_client.h 
        iothub_client_authorization.h
        iothub_client_diagnostic.h
        iothub_client_ll.h
        Iothub_client_options.h
        iothub_client_private.h
        iothub_client_retry_control.h
        iothub_client_sample_mqtt_esp8266.h
        Iothub_client_version.h
        iothub_message.h
        iothub_transport_ll.h
        iothubtransport.h
        iothubtransport_mqtt_common.h
        Iothubtransportmqtt.h

4.  Copy following files from **azure-c-shared-utility** to **esp8266\_rtos\_sdk/third_partty/azure\_c\_shared\_utility**

        Agenttime.c 
        agenttime_esp8266.c
        base64.c buffer.c
        constbuffer.c
        crt_abstractions.c
        Doublylinkedlist.c
        gballoc.c
        hmac.c
        hmacsha256.c
        map.c optionhandler.c
        platform_esp8266.c
        Sastoken.c
        sha1.c
        sha224.c
        sha384-512.c
        singlylinkedlist.c
        string_tokenizer.c
        strings.c
        Threadapi_esp8266.c
        tickcounter_esp8266.c
        tlsio_ssl_esp8266.c
        urlencode.c
        usha.c
        vector.c
        Version.c
        xio.c
        xlogging.c

    and build own makefile (makefile file can refer to the makefile of its library under third_partty)

5.  Copy these files from **azure-c-shared-utility** to **esp8266_rtos_sdk/include/azure_c_shared_utility**

        Agenttime.h
        base64.h
        buffer_.h
        consolelogger.h
        constbuffer.h
        crt_abstractions.h
        doublylinkedlist.h
        gballoc.h 
        Hmac.h
        hmacsha256.h
        lock.h
        macro_utils.h
        map.h
        optimize_size.h
        optionhandler.h
        platform.h
        refcount.h
        refcount_os.h 
        Sastoken.h
        sha.h
        sha-private.h
        shared_util_options.h
        singlylinkedlist.h
        socketio.h
        string_tokenizer.h
        String_tokenizer_types.h
        strings.h
        strings_types.h
        threadapi.h
        tickcounter.h
        tlsio.h
        tlsio_openssl.h
        umock_c_prod.h
        Urlencode.h
        vector.h
        vector_types.h
        vector_types_internal.h
        xio.h 
        xlogging.h

6.  Copy these files from **azure_umqtt_c** to **esp8266\_rtos\_sdk/third_partty/azure\_umqtt\_c**

        Mqtt_message.c mqtt_client.c mqtt_codec.c

    and build your own makefile (makefile file can refer to the makefile of its library under third_partty)

7.  Copy these files from **azure_umqtt_c** to **esp8266_rtos_sdk/include/azure_umqtt_c**

        Mqtt_message.h mqtt_client.h mqtt_codec.h mqttconst.h

8.  Modify the file **esp8266\_rtos\_sdk/third\_party/ssl/ssl/ssl\_os\_port.c**:

    Delete line 316: *gettimeofday function*

9.  Modify the file **esp8266\_rtos\_sdk/third\_party/ssl/ssl/ssl\_tls1\_clnt.c**: 

    Delete line 98: *send\_cert\_verify (SSL)*;

10. Modify the file **iothub\_client\_sample\_mqtt\_esp8266.c**:

    Delete line 63: (void)printf("Received Message [%d]\r\n Message ID: %s\r\n Correlation ID: %s\r\n Data: <<<%.*s>>> & Size=%d\r\n", *counter, messageId, correlationId, (int)size, buffer, (int)size);

	Add code in line 63:
	
        char *bufferTmp = (char *)malloc(size+1);
        if(NULL != bufferTmp )
        {
            memset(bufferTmp, 0, size+1);
            memcpy(bufferTmp, buffer, size);
            (void)printf("Received Message [%d]\r\n Message ID: %s\r\n Correlation ID: %s\r\n Data: <<<%s>>> & Size=%d\r\n", *counter, messageId, correlationId, bufferTmp, (int)size);
            free(bufferTmp);
        }
        
    Annotation:Because it doesn't support printf "%.*s"

11. Modify the file **iothub\_client\_sample\_mqtt\_esp8266.c**:

	Delete line 20: static const char* connectionString = "[device connection string]";
	Add code in line 20: static const char* connectionString = "HostName=hxhtest.azure-devices.cn;DeviceId=deviceec91fcfe8d974ba480ff1fb8caf0b617;SharedAccessKey=*******";

	Annotation:The Content of connectionString is from Azure IoT Hub

12.  Modify the file **esp8266\_rtos\_sdk/third\_party/Makefile**

    Add fields:

        CCFLAGS += -std=c99
        CCFLAGS += -D_WIN32=0
        CCFLAGS += -DFREERTOS_ARCH_ESP8266=1

13.  Modify the file **esp8266\_rtos\_sdk/Makefile:**

    (1) Remove field: *-Werror*

    (2) Add fields:

        INCLUDES -I = $(SDK_PATH) /include/azure_c_shared_utility
        INCLUDES -I = $(SDK_PATH) /include/azure_umqtt_c
        INCLUDES -I = $(SDK_PATH) /include/iothub_client

14.  Compiling lib

    -   Enter esp8266_rtos_sdk/third_partty
	-   Run ./make_lib.sh ssl, generating libssl.a under esp8266_rtos_sdk/lib
	-   Run ./make_lib.sh azure_umqtt_c, generating libazure_umqtt_c.a under esp8266_rtos_sdk/lib
	-   Run ./make_lib.sh azure_c_shared_utility, generating libazure_c_shared_utility.a under esp8266_rtos_sdk/lib
	-   Run ./make_lib.sh iothub_client, generating libiothub_client.a under esp8266_rtos_sdk/lib


##4. Compiling application

1.  Modify the file **esp8266\_rtos\_sdk/application/Makefile**

    (1) -lminic is replaced by -lcirom -lmirom
    (2) Add field at LINKFLAGS_eagle.app.v6

        -lazure_umqtt_c \
        -lazure_c_shared_utility \
        -liothub_client \

2.  Modify **user_main.c** in application

    (1) The header file contains **#include "iothub\_client\_sample\_mqtt\_esp8266.h"**
    (2) Create a task, call **iothub\_client\_sample\_mqtt\_esp8266\_run ();**

##5.  Compiling file of bin

    -   Enter **esp8266\_rtos\_sdk**
    -   run **./vane\_turbo\_app\_compile.sh**
    -   compile and generate bin file: **esp8266\_rtos\_sdk/bin/vane\_bin/user1.1024.new.2.bin**

##6. Run

1. Through the **ESP\_DOWNLOAD\_TOOL\_V2.4.exe** tool, download it to the **VL-WFP-P-8266** module.

2. Automatic run after power up.

3. Send message "hello word" to device by Device Explorer.

4. See receiving information on the Device Explorer and  see the receiving information and sending information on the Serial port printing of device.

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

