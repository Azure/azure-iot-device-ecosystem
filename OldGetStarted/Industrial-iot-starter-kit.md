---
platform: windows 10 iot enterprise 2016 ltsb 1609
device: industrial iot starter kit
language: csharp
---

[Industrial IoT Starter Kit](https://data-intelligence.softing.com/products/iot-gateways/industrial-iot-starter-kit/) running Windows 10 IoT Enterprise 2016 LTSB 1609
===
---

# Table of Contents

-   [Industrial IoT Starter Kit](#industrial-iot-starter-kit)
-   [Connect the sensors](#connect-the-sensors)
-   [Build and Run the sample](#build-and-run-the-sample)
-   [Next steps](#next-steps)

# Industrial IoT Starter Kit

The Industrial IoT Starter Kit includes:

-   [softing iisk gl20](../../../get_started/softing-iisk-gl20-csharp.md)
-   [dataFEED Suite](https://data-intelligence.softing.com/products/software-platform/datafeed-opc-suite/)
-   Docker-ce for Windows

# Connect the sensors 

The [dataFEED Suite](https://data-intelligence.softing.com/products/software-platform/datafeed-opc-suite/) allows to connect to different kinds of PLCs, as listed in the technical [details section](https://data-intelligence.softing.com/products/software-platform/datafeed-opc-suite/#tx-dftabs-tabContent1).
Please read the manual from the [download section](https://data-intelligence.softing.com/products/software-platform/datafeed-opc-suite/#tx-dftabs-tabContent2).  
Please read also the manual of the Industrial IoT Starter Kit from the [download section](https://data-intelligence.softing.com/products/iot-gateways/industrial-iot-starter-kit/#tx-dftabs-tabContent2).

# Build and Run the sample

-   Download the [Azure IoT SDK](https://github.com/Azure/azure-iot-sdk-csharp) and the sample programs and save them to a local repository.
-   Start a new instance of Visual Studio 2017.
-   Open the **iothub\_csharp\_client.sln** solution in the `device` folder in the local copy of the repository.
-   Enable developer mode in Windows 10 settings:

    ![developer-mode](media/developer-mode.png)

-   In Visual Studio, from Solution Explorer, navigate to the **samples** folder.
-   In the **DeviceClientAmqpSample** project, open the ***Program.cs*** file.
-   Locate the following code in the file:

        private const string DeviceConnectionString = "<replace>";

-   Replace `<replace>` with the connection string for the device.
-   In **Solution Explorer**, right-click the **DeviceClientAmqpSample** project, click **Debug**, and then click **Start new instance** to build and run the sample. The console displays messages as the application sends device-to-cloud messages to IoT Hub.
-   Use the **DeviceExplorer** utility to observe the messages IoT Hub receives from the **Device Client AMQP Sample** application.
-   Refer "Monitor device-to-cloud events" in [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to see the data the device is sending.
-   Refer "Send cloud-to-device messages" in [DeviceExplorer Usage document](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) for instructions on sending messages to device.

# Next steps

Read the manual from the [download section](https://data-intelligence.softing.com/products/iot-gateways/industrial-iot-starter-kit/#tx-dftabs-tabContent2) to use the Azure preconfigured solution [Connected Factory](https://docs.microsoft.com/en-us/azure/iot-suite/iot-suite-connected-factory-overview).

Because The [Industrial IoT Starter Kit](https://data-intelligence.softing.com/products/iot-gateways/industrial-iot-starter-kit/) allows you to use docker containers, you could also use the Starter Kit as an Azure edge device.
Please take a look at following tutorials:

- [Quickstart: Deploy your first IoT Edge module from the Azure portal to a Windows device - preview](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart)
- [Deploy Azure IoT Edge on a simulated device in Windows - preview](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-simulate-device-windows)
