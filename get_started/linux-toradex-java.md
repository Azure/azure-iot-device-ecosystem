---
platform: linux
device: toradex modules
language: java
---

Run a simple Java sample on Toradex Modules device running Linux
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

This document describes how to connect Toradex Modules with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Use the [Azure IoT Java Application Development guides][java-dev-guides] to build example Java applications (e.g. send-event.java).
-   Ensure the module is flashed with [Toradex V2.5 Linux image or newer][toradex_image_update].

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

1.  Insert the module into a compatible carrier board.  Power on the system and connect to the internet.

2.  Use the command line to install OpenJDK and necessary dependencies:

    ```
    wget -P /etc/opkg/ http://feeds.toradex.com/extra/2.5/conf/all/extra.conf
    opkg update
    opkg install openjdk-7-jre openssl
    ```

3.  Import trusted root certificates:

    ```
    wget https://www.geotrust.com/resources/root_certificates/certificates/Equifax_Secure_Certificate_Authority.pem
	wget https://cacert.omniroot.com/bc2025.crt
	/usr/lib/jvm/java-7-openjdk/jre/bin/keytool -keystore ~/cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.pem
	/usr/lib/jvm/java-7-openjdk/jre/bin/keytool -keystore ~/cacerts -importcert -alias baltimorecybertrustca -file bc2025.crt
	/usr/lib/jvm/java-7-openjdk/jre/bin/keytool -list -keystore ~/cacerts
	```

<a name="Build"></a>
# Step 3: Build and Run the sample

1.  Transfer the compiled Java example applications to the device.

2.  On the host machine, start a new instance of the [Device Explorer][device-explorer], select or create a new device, obtain and note the connection string for the device, and begin monitoring under the Data tab.

3.  Back on the device, navigate to the folder containing the executable JAR file for the sample that you wish to run and run the samples as follows:

	The executable JAR file for sending event
	can be found at:

	```
	{IoT device SDK root}/java/device/samples/send-event/target/send-event-{version}-with-deps.jar
	```

	Navigate to the directory with the event sample. Run the sample using the following command:

	```
	java -jar ./send-event-{version}-with-deps.jar "{connection string}" "{number of requests to send}" "{https or amqps}"
	```

	The executable JAR file for receiving messages can be found at:

	```
	{IoT device SDK root}/java/device/samples/handle-messages/target/handle-messages-{version}-with-deps.jar
	```

	Navigate to the directory with the messages sample. Run the sample using the following command:

	```
	java -jar ./handle-messages-{version}-with-deps.jar "{connection string}" "{https or amqps}"
	```

	Note that the double quotes around each argument are required, but the braces '{' and '}' should be removed.

## Next Steps
To learn how to create a Java application that communicates with an IoT hub from scratch, see [Get started with Azure IoT Hub for Java][how-to-build-a-java-app-from-scratch].

## Documentation

The documentation can be found [here](https://azure.github.io/azure-iot-sdk-java/device/api_reference/index.html).

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
[java-dev-guides]: https://github.com/Azure/azure-iot-sdk-java/blob/master/device/readme.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/readme.md
[how-to-build-a-java-app-from-scratch]: https://azure.microsoft.com/documentation/articles/iot-hub-java-java-getstarted/

[toradex_image_update]: http://developer.toradex.com/knowledge-base/how-to-setup-environment-for-embedded-linux-application-development#Linux_Image_Update

