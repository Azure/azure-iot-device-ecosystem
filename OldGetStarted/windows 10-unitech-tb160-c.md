---
platform: windows 10 iot enterprise ltsb
device: unitech tb160
language: C
---

Run a simple C sample on unitech TB160 running Windows 10 IoT Enterprise LTSB
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

This document describes how to connect unitech TB160 with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:



<a name="Setup_DevEnv"></a>
-   [Prepare your development environment][devbox_setup]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   unitech TB160

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device



-  Turn on your unitech TB160 and connect to internet



<a name="Build"></a>
# Step 3: Build SDK and Run the sample
Please find Azure IoT SDKs for C sample code [here][c-sample-code].



1.  Start a new instance of Visual Studio 2015 and open project from here:

        azure-iot-sdk-c\iothub_client\samples\iothub_client_sample_http\windows\

2.  Go to **iothub_client_sample_http.c**, replace the **[device connection string]** placeholder with connection string of the device you have created in [Provision your device and get its credentials][lnk-manage-iot-hub] and save the file.  An example of IoT Hub Connection String is as below:

         HostName=[YourIoTHubName];SharedAccessKeyName=[YourAccessKeyName];SharedAccessKey=[YourAccessKey]

3. See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

4. In **Solution Explorer**, right-click the **iothub_client_sample_http** project, click **Debug**, and then click **Start new instance** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.

5. See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.



<a name="NextSteps"></a>
# Next Steps
You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

- [Manage cloud device messaging with iothub-explorer][iot-hub-explorer-cloud-device-messaging]
- [Save IoT Hub messages to Azure data storage][iot-hub-store-data-in-azure-table-storage]
- [Use Power BI to visualize real-time sensor data from Azure IoT Hub][iot-hub-live-data-visualization-in-power-bi]
- [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub][iot-hub-live-data-visualization-in-web-apps]
- [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning][iot-hub-weather-forecast-machine-learning]
- [Remote monitoring and notifications with Logic Apps][iot-hub-monitoring-notifications-with-azure-logic-apps]




[iot-hub-explorer-cloud-device-messaging]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[iot-hub-store-data-in-azure-table-storage]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[iot-hub-live-data-visualization-in-power-bi]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[iot-hub-live-data-visualization-in-web-apps]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[iot-hub-weather-forecast-machine-learning]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[iot-hub-monitoring-notifications-with-azure-logic-apps]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[devbox_setup]: https://catalog.azureiotsuite.com/docs?title=Azure/azure-iot-sdk-c/doc/devbox_setup
[lnk-setup-iot-hub]: https://catalog.azureiotsuite.com/docs?title=Azure/azure-iot-device-ecosystem/setup_iothub
[lnk-manage-iot-hub]: https://catalog.azureiotsuite.com/docs?title=Azure/azure-iot-device-ecosystem/manage_iot_hub
[c-sample-code]:https://github.com/Azure/azure-iot-sdk-c/
[mainactivity-source-code]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/samples/android-sample/app/src/main/java/com/iothub/azure/microsoft/com/androidsample/MainActivity.java