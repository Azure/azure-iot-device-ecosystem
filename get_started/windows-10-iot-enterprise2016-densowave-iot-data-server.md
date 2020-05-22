---
platform: windows 10 iot enterprise 2016
device: iot data server
language: csharp
---

Connect IoT Data server to your Azure IoT Hub
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

This document describes how to connect IoT Data Server to Azure Iot Hub. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   IoT Data Server device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   If you use HTTP protocol to communicate with IoT Hub via proxy, you have to configure proxy. Select `System settings`-`Network`-`Proxy`, and input your proxy server and port number.

<a name="Build"></a>
# Step 3: Build and Run the sample

-   Click `Project Edit`

![](./media/densowave-iot-data-server/01.png)

-   Right-click `project` in the lefthand window. Select `Controller setting wizard`.

-   Select `Manufactuer`-`Microsoft`-`Azure IoT Provider`, and click `Next`.

![](./media/densowave-iot-data-server/02.png)

-   Input 'Controller name' and 'IoT Hub Connection String' and select 'Protocol Name'. Click 'Next'.

![](./media/densowave-iot-data-server/03.png)

-   input `Item name` , check `Same as item` and click `add`. Click `Finish`.

![](./media/densowave-iot-data-server/04.png)

-  Click your item, and change attribute from `Read` to `Write`.

![](./media/densowave-iot-data-server/05.png)

-  Create another controller, right-click and select `Trigger settings`. Click `Action...`

![](./media/densowave-iot-data-server/06.png)

-  Select `Output JSON`, and click `Settings...`

![](./media/densowave-iot-data-server/07.png)

-  Select Azure IoT Hub Controller Item in `Result store item`, and create JSON formatted message.

![](./media/densowave-iot-data-server/08.png)

-  Start project.

![](./media/densowave-iot-data-server/09.png)

-   Use the **DeviceExplorer** utility to observe the messages IoT Hub receives from the **Device Client Sample** application.
-   Refer "Monitor device-to-cloud events" in [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to see the data your device is sending.
-   Refer "Send cloud-to-device messages" in [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) for instructions on sending messages to device.

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
[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
