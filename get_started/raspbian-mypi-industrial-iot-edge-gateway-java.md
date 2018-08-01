---
platform: raspbian
device: mypi industrial iot edge gateway
language: java
---

Run a simple Java sample on MyPi Industrial IoT Edge Gateway device
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Command Line Arguments](#command_line_arguments) 

<a id="Introduction"></a>
# Introduction

**About this document**

This document describes how to build and run the Java sample application. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a id="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][devbox-setup]
-   Computer with Git client installed and access to the
    [azure-iot-sdk-java](https://github.com/Azure/azure-iot-sdk-java) GitHub public repository.
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   MyPi Industrial IoT Edge Gateway device.

<a id="PrepareDevice"></a>
# Step 2: Prepare your Device
-   Follow the instructions from [device website](http://www.embeddedpi.com/documentation).

<a id="Build"></a>
# Step 3: Build and Run the sample

If you have successfully [prepared your development environment][devbox-setup] the IoT device SDK and all samples should be built and ready to use.

-   Start a new instance of the [Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md), select or create a new device, obtain and note the connection string for the device, and begin monitoring under the Data tab.

-   Build the client library along with the **samples** :

        {IoT device SDK root}/java/device/>mvn install -DskipTests

    On build completion, you will see summary of what got built.    

-   Navigate to the folder containing the executable JAR file for the sample that you wish to run and run the samples as follows:

    For example, the executable JAR file for sending and receving event, **send-receive-sample** can be found at:


        {IoT device SDK root}/java/device/samples/send-receive-sample/target/send-event-{version}-with-deps.jar

    Navigate to the directory with the sample. Run the sample using the following command:


        java -jar ./send-receive-sample-{version}-with-deps.jar "{connection string}" "{number of requests to send}" "{https or amqps or mqtt or amqps_ws }"

    Note that the double quotes around each argument are required, but the braces '{' and '}' should be removed.

<a id="command_line_arguments"></a>
## More details on command line arguments
Samples would use following command line arguments:

-   [Device connection string] - `HostName=<iothub_host_name>;DeviceId=<device_id>;SharedAccessKey=<device_key>`

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
2. [Number of requests to send]: For example, **5** 

3. [`https | amqps | mqtt | amqps_ws`]: For example, amqps_ws (AMQP over WebSocket)

4. [Path to certificate to enable 1-way authentication]: For example, `azure-iot-sdk-c\tree\master\certs\ms.der` **optional argument**

Path to certificate is an **optional** argument and would be needed in case you want to point it to the local copy of the Server side certificate. Please note that this option is used by client for validating Root CA sent by Azure IoT Hub Server as part of TLS handshake. It is for 1-way TLS authentication and is **not** for specifying client side certificate (2-way TLS authentication).

-   To learn how to create a Java application that communicates with an IoT hub from scratch, see [Get started with Azure IoT Hub for Java][how-to-build-a-java-app-from-scratch].

## Documentation

The documentation can be found [here](https://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html).

[devbox-setup]: java-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[how-to-build-a-java-app-from-scratch]: https://azure.microsoft.com/documentation/articles/iot-hub-java-java-getstarted/