---
platform: Windows 10 Pro
device: faytech-101J1900CAP
language: csharp
---

Run a simple Csharp sample on [faytech-101J1900CAP](https://www.faytech.com/catalogue/product/101-capacitive-touch-pc-ft101j1900w4g64gcap/) device running Windows 10 Pro
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

This document describes how to connect faytech-101J1900CAP device running Windows 10 Pro with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-windows]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   [faytech-101J1900CAP device](https://www.faytech.com/catalogue/product/101-capacitive-touch-pc-ft101j1900w4g64gcap/)

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Intsall the Windows 10 Pro on your device
-	Install properly all the hardware drivers
-	Download and install the [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) 

https://download.faytech.com/iothub/

<a name="Build"></a>
# Step 3: Build and Run the sample
-	### Build from the source code
	-   Download the [Azure IoT SDK](https://github.com/Azure/azure-iot-sdk-csharp) and the sample programs and save them to your local repository.
	-	Install the Visual Studio 2015 (you can also use the Visual Studio 2017).
	-	Navigate to the sample programs you downloaded already and open the  **iothub_csharp_client.sln** project file with your Visual Studio program.
	-	When the project is loaded into your Visual Studio, open the **DeviceClientHttpSample** project, edit the **Program.cs** source file :
		-	Comment the line number 17 
		-	replace the line number 28:
	
				DeviceClient deviceClient = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Http1)
			
			by:
		
				DeviceClient deviceClient = DeviceClient.CreateFromConnectionString(args[0], TransportType.Http1)
			
		-	Save and compile/build the project
			
	-   Open a device console (command prompt or a powershell window) and change to your local SDK **azure-iot-sdk-csharp** directory.
	-	Run the command `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned` in order to enable executing Powershell scripts on your system.
        
	- From the device Powershell console, run the sample using following command:

			cd iothub\device\samples\DeviceClientHttpSample\bin\Debug\netcoreapp2.0
			dotnet DeviceClientHttpSample.dll <the_device_connexion_string_here>

-	### Use the built **DeviceClientHttpSample.dll** program
	-	Download the archive file `IoTHub_FAY101J1900CAP.rar` from [faytech's downloading space](https://download.faytech.com/iothub/) and extract the content.
	-   Open a powershell window and change to your extracted repository using the command `cd`
	-	Run the command `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned` in order to enable executing Powershell scripts on your system.
	-	Execute now the sample program by typing:
		
			dotnet DeviceClientHttpSample.dll <the_device_connexion_string_here>
			
	
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
[lnk-setup-iot-hub]: ../../setup_iothub.md
[lnk-manage-iot-hub]: ../../manage_iot_hub.md
