---
platform: wince2013
device: toradex-module
language: c
---

Run a simple C sample on Windows Embedded Compact 2013 on a Toradex module
===
---

# Table of Contents

- [Introduction](#Introduction)
- [Step 1: Prerequisites](#Step-1-Prerequisites)
- [Step 2: Prepare your Device](#Step-2-PrepareDevice)
- [Step 3: Build and Run the Sample](#Step-3-Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to build and run the **simplesample_http** application on the Windows Embedded Compact 2013 platform. This multi-step process includes:
- Configuring Azure IoT Hub
- Registering your IoT device
- Build and deploy Azure IoT SDK on device

<a name="Step-1-Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:
- Computer with Git client installed and access to the
  [azure-iot-sdk-c](https://github.com/Azure/azure-iot-sdk-c) GitHub public repository.
- [Colibri Modules][colibri-modules] running the [Windows Embedded Compact 2013 image provided by Toradex][toradex-images]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

<a name="Step-2-PrepareDevice"></a>
# Step 2: Prepare your Device

These instructions assume that you have done the following:

- [Prepare your development environment][devbox-setup]

These instructions apply to [Colibri Modules][colibri-modules] running the [Windows Embedded Compact 2013 image provided by Toradex][toradex-images], if you are using a different operating system image or a different device running Windows Embedded Compact 2013, some of the steps described in this document may require different operations on your target device.

<a name="Step-3-Build"></a>
# Step 3: Build and Run the sample

1. Start a new instance of Visual Studio 2015. Open the **azure_iot_sdks.sln** solution in the **cmake_ce8** folder in your home directory.

2. In Visual Studio, in **Solution Explorer**, navigate to **iothub_client\samples\iothub_client_sample_http** project, open the **iothub_client_sample_http.c** file.

3. Locate the following code in the file:

    ```
   static const char* connectionString = "[device connection string]";
    ```

4. Replace "[device connection string]" with the device connection string you noted [earlier](#Step-1-Prerequisites) and save the changes:

  ```
  static const char* connectionString = "HostName=..."
  ```

5. Check that date and time on your device are set correctly. Using a date outside of the validity period of your certificates can prevent your device from getting connected to the hub. To update your date and time you can open a command prompt and type:
```
  services refresh NTP0:
```  
6. In **Solution Explorer**, right-click the **iothub_client_sample_http** project, click **Properties**, then select **Configuration Properties\Debugging** to build and then insert the ip address of your target device in the **IP Address** field. Select **OK** to save your changes.

7. In **Solution Explorer**, right-click the **iothub_client_sample_http** project, click **Debug**, and then click **Start new instance** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.

8. See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the **iothub_client_sample_http** application and how to send cloud-to-device messages to the **iothub_client_sample_http** application.

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
[devbox-setup]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/readme.md
[colibri-modules]:https://www.toradex.com/computer-on-modules/colibri-arm-family
[toradex-images]:http://developer.toradex.com/frequent-downloads#Windows_Embedded_Compact

[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

