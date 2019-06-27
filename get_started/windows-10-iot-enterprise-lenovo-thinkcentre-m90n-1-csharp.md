---
platform: windows 10 iot enterprise
device: lenovo thinkcentre m90n-1
language: csharp
---

Deploy an IoT Edge Module on Lenovo ThinkCentre M90n-1 device running Windows 10 IoT Enterprise.
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Deploy your first IoT Edge module from the Azure to Lenovo ThinkCentre M90n-1 device](#Manual)
-   [Step 4: View generated data](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**
 The Azure IoT Edge modules can be used to deploy code that implements your business logic directly to your IoT Edge devices. This step-by-step guide will allow you to Use Visual Studio Code to develop your first IoT Edge modules and deploy it into a ThinkCentre M90n-1 device running Windows 10 IoT Enterprise from IoT Hub remotely.

<a name="Prerequisites"></a>
# Step 1: Prerequisites

By completing either of those tutorials, you should have the following prerequisites in place:

-   [Setup your IoT hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   A ThinkCentre M90n-1 device running Windows 10 IoT Enterprise as Target Deployment device.
-   [Register a new Azure IoT Edge device from the Azure portal](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-register-device-portal)

<a name="PrepareDevice"></a>
# Step 2: Prepare your M90n-1 device

The steps in this section all take place on your ThinkCentre M90n-1 device.

Use PowerShell to download and install the IoT Edge runtime. Use the device connection string that you retrieved from IoT Hub to configure your device.

1.  If you haven't already, follow the steps in [Register a new Azure IoT Edge device to register your device](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-register-device-portal) and retrieve the device connection string.
2.  Run PowerShell as an administrator.

        {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
        Install-SecurityDaemon -ContainerOs Windows -DeviceConnectionString "{CONNECTION_STRING}"

**Using The Real Connection String** 

Important: remember to replace the "{CONNECTION_STRING}" string in the above snippet with the actual connection string. To understand different connection strings in Azure IoT Hub, you can refer to [devblogs](https://devblogs.microsoft.com/iotdev/understand-different-connection-strings-in-azure-iot-hub/).

This string gives you access to the service, so remember to keep it secret -- in particular, you don't want to check it into your source control system, especially if you project is open source.

To verify that the runtime is successfully installed and configured, you can check the status of the IoT Edge service:

    Get-Service iotedge
	iotedge list

 ![](./media/m90n-1/iotedge-list-1.png)

<a name="Manual"></a>
# Step 3: Deploy a module

The following steps shows how to manage your Azure IoT Edge device from the cloud to deploy a module that sends telemetry data to IoT Hub.

One of the key capabilities of Azure IoT Edge is being able to deploy code to your IoT Edge devices from the cloud. IoT Edge modules are executable packages implemented as containers.
 
In this section, you deploy a pre-built module from the IoT Edge Modules section of the Azure Marketplace. 

<a name="Step-3-1-IoTEdgeRunTime"></a>
## Select Your Device

1.  Sign in to the Azure portal and navigate to your IoT hub.
2.  Select IoT Edge from the menu.
3.  Click on the ID of the target device you created in Step 2 from the list of devices.
4.  Select Set Modules from the bellowing menu.

<a name="Step-3-2-IoTEdgeRunTime"></a>
## Configure a deployment manifest

The Azure portal has a wizard that walks you through creating the deployment manifest, instead of building the JSON document manually. It has three steps: **Add modules**, **Specify routes**, and **Review deployment**.

**Add modules:**

1.  In the Deployment modules section of the page, select **Add**.
2.  Look at the types of modules from the drop-down list:"IoT Edge Module" and "Azure Stream Analytics Module".
3.  Select the IoT Edge Module.
4.  Provide a name for the module, then specify the container image. Here we use an example module from Microsoft. 
	1.  **Name** - tempSensor
	2.  **Image URI** - mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0
5.  Fill out the optional fields if necessary. Here just leave it alone.
6.  Select **Save**.
7.  Select **Next** to continue to the routes section.

**Specify routes**

By default the wizard gives you a route called route and defined as 

	{
  		"routes": {
    		"route": "FROM /messages/* INTO $upstream"
  		}
	}

which means that any messages output by any modules are sent to your IoT hub.


**Review deployment**

The review section shows you the JSON deployment manifest that was created based on your selections in the previous two sections. Note that there are two modules declared that you didn't add: **$edgeAgent** and **$edgeHub**. These two modules make up the IoT Edge runtime and are required defaults in every deployment.

Review your deployment information, then select **Submit**.

**View modules on your device**
Once you've deployed modules to your device, you can view all of them in the Device details page of the portal. This page displays the name of each deployed module, as well as useful information like the deployment status and exit code.

<a name="NextSteps"></a>
# Step 4: View generated data
In this quickstart, you registered an IoT Edge device and installed the IoT Edge runtime on it. Then, you used the Azure portal to deploy an IoT Edge module to run on the device without having to make changes to the device itself.

To confirm that the module deployed from the cloud is running on your IoT Edge device:

    iotedge list


 ![](./media/m90n-1/iotedge-list-2.png)

and you could view the messages being sent from the temperature sensor module to the cloud:

    iotedge logs tempSensor -f


 ![](./media/m90n-1/iotedge-logs.png)

  
[setup-devbox-Windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md