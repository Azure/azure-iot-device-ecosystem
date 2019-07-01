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
-   [Step 4: Create and Deploy your own IoT Edge Module](#Create)

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

To develop your IoT Edge module, install the following additional prerequisites on your **develop machine**:

-   Visual Studio 2019 configured with the [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) in your develop machine.
-   [Docker CE](https://docs.docker.com/docker-for-windows/) configured to run **Windows containers**.
-   Set up A container registry, like [Azure Container Registry.](https://docs.microsoft.com/azure/container-registry/)

This tutorial teaches the development steps for Visual Studio 2019. If you are using Visual Studio 2017, plrease download and install [Azure IoT Edge Tools for Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools).

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

 ![](./media/m90n-1/deploy_manifest.png)

Review your deployment information, then select **Submit**.

**View modules on your device**

Once you've deployed modules to your device, you can view all of them in the Device details page of the portal. This page displays the name of each deployed module, as well as useful information like the deployment status and exit code.


**View generated data**

In this quickstart, you registered an IoT Edge device and installed the IoT Edge runtime on it. Then, you used the Azure portal to deploy an IoT Edge module to run on the device without having to make changes to the device itself.

To confirm that the module deployed from the cloud is running on your IoT Edge device:

    iotedge list


 ![](./media/m90n-1/iotedge-list-2.png)

and you could view the messages being sent from the temperature sensor module to the cloud:

    iotedge logs tempSensor -f


 ![](./media/m90n-1/iotedge-logs.png)

<a name="Create"></a>
# Step 4:  Create and Deploy your own IoT Edge Module

The following steps create an IoT Edge module project by using Visual Studio and the Azure IoT Edge Tools extension. Here shows an example module which filters out the message coming from the simulated temprecture sensor module we deployed in step 3.

## 4.1 Create a module project

The Azure IoT Edge Tools provides project templates for all supported IoT Edge module languages in Visual Studio. These templates have all the files and code that you need to deploy a working module to test IoT Edge, or give you a starting point to customize the template with your own business logic.

1.  Launch Visual Studio 2019 and select Create New Project.
2.  In the new project window, search IoT Edge project and choose the **Azure IoT Edge (Windows amd64)** project. Click **Next**.

    ![](./media/m90n-1/azure_iot_project_win64.png)

3.  In the configure your new project window, name the project and solution as **" CSharpTutorialApp"**. Click **Create** to create the project.
4.  In the IoT Edge application and module window, Select **C# Module** and rename it as **"TempFilterModule"**

    ![](./media/m90n-1/application-and-module.png)

**Using The Real container registry**

An image repository includes the name of your container registry and the name of your container image. Your container image is prepopulated from the module project name value. Replace **localhost:5000** with the login server value from your Azure container registry. You can retrieve the login server from the Overview page of your container registry in the Azure portal. 

## 4.2 Update the module with custom code

Let's add some additional code so that the module processes the messages at the edge before forwarding them to IoT Hub. Update the module so that it analyzes the temperature data in each message, and only sends the message to IoT Hub if the temperature exceeds a certain threshold.

1.  In Visual Studio, open **TempFilterModule** > **Program.cs**.
2.  At the top of the **TempFilterModule**  namespace, add three **using** statements for types that are used later:

        using System.Collections.Generic; // For KeyValuePair<>
        using Microsoft.Azure.Devices.Shared; // For TwinCollection
        using Newtonsoft.Json;// For JsonConvert

3.  Add the **temperatureThreshold** variable to the **Program** class after the counter variable. The **temperatureThreshold** variable sets the value that the measured temperature must exceed for the data to be sent to the IoT hub.

    	static int temperatureThreshold { get; set; } = 25;

4.  Add the **MessageBody**, **Machine**, and **Ambient** classes to the Program class after the variable declarations. These classes define the expected schema for the body of incoming messages.

    	class MessageBody
    	{
    		public Machine machine {get;set;}
    		public Ambient ambient {get; set;}
    		public string timeCreated {get; set;}
    	}
    	class Machine
    	{
    		public double temperature {get; set;}
    		public double pressure {get; set;} 
    	}
    	class Ambient
    	{
    		public double temperature {get; set;}
    		public int humidity {get; set;} 
    	}


5.  Find the **Init** method. This method creates and configures a **ModuleClient** object, which allows the module to connect to the local Azure IoT Edge runtime to send and receive messages. The code also registers a callback to receive messages from an IoT Edge hub via the **input1** endpoint.

    Replace the entire Init method with the following code:


        static async Task Init()
	    {
	    	AmqpTransportSettings amqpSetting = new AmqpTransportSettings	(TransportType.Amqp_Tcp_Only);
	    	ITransportSettings[] settings = { amqpSetting };
	    
	    	// Open a connection to the Edge runtime
	    	ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
	    	await ioTHubModuleClient.OpenAsync();
	    	Console.WriteLine("IoT Hub module client initialized.");
	    
	    	// Read the TemperatureThreshold value from the module twin's desired properties
	    	var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
	    	await OnDesiredPropertiesUpdate(moduleTwin.Properties.Desired, ioTHubModuleClient);
	    
	    	// Attach a callback for updates to the module twin's desired properties.
	    	await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertiesUpdate, null);
	    
	    	// Register a callback for messages that are received by the module.
	    	await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
        }

    
	This updated Init method still sets up the connection to the IoT Edge runtime with the ModuleClient, but also adds new functionality. It reads the module twin's desired properties to retrieve the **temperatureThreshold** value. Then, it creates a callback that listens for any future updates to the module twin's desired properties. With this callback, you can update the temperature threshold in the module twin remotely, and the changes will be incorporated into the module.

	The updated Init method also changes the existing **SetInputMessageHandlerAsync** method. In the sample code, incoming messages on input1 are processed with the **PipeMessage** function, but we want to change that to use the **FilterMessages** function that we'll create in the following steps.

6.  Add a new **onDesiredPropertiesUpdate** method to the **Program** class. This method receives updates on the desired properties from the module twin, and updates the **temperatureThreshold** variable to match. All modules have their own module twin, which lets you configure the code that's running inside a module directly from the cloud.
    	
        static Task OnDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
        {
           	try{
    			Console.WriteLine("Desired property change:");
    			Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
    	
    			if (desiredProperties["TemperatureThreshold"]!=null)
    			temperatureThreshold = desiredProperties["TemperatureThreshold"];
    	
    		}
    		catch (AggregateException ex){
    			foreach (Exception exception in ex.InnerExceptions){
    				Console.WriteLine();
    				Console.WriteLine("Error when receiving desired property: {0}", exception);
    			}
    		}
    		catch (Exception ex){
    			Console.WriteLine();
    			Console.WriteLine("Error when receiving desired property: {0}", ex.Message);
    		}
    		return Task.CompletedTask;
    	}

7.  Remove the sample **PipeMessage** method and replace it with a new **FilterMessages** method. This method is called whenever the module receives a message from the IoT Edge hub. It filters out messages that report temperatures below the temperature threshold set via the module twin. It also adds the **MessageType** property to the message with the value set to **Alert**.

	    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
		{
			var counterValue = Interlocked.Increment(ref counter);
			try
			{
				ModuleClient moduleClient = (ModuleClient)userContext;
				var messageBytes = message.GetBytes();
				var messageString = Encoding.UTF8.GetString(messageBytes);
				Console.WriteLine($"Received message {counterValue}: [{messageString}]");
		
				// Get the message body.
				var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);
		
				if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
				{
					Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
						$"exceeds threshold {temperatureThreshold}");
					var filteredMessage = new Message(messageBytes);
					foreach (KeyValuePair<string, string> prop in message.Properties)
					{
						filteredMessage.Properties.Add(prop.Key, prop.Value);
					}
		
					filteredMessage.Properties.Add("MessageType", "Alert");
					await moduleClient.SendEventAsync("output1", filteredMessage);
				}
		
				// Indicate that the message treatment is completed.
				return MessageResponse.Completed;
			}
			catch (AggregateException ex)
			{
				foreach (Exception exception in ex.InnerExceptions)
				{
					Console.WriteLine();
					Console.WriteLine("Error in sample: {0}", exception);
				}
				// Indicate that the message treatment is not completed.
				var moduleClient = (ModuleClient)userContext;
				return MessageResponse.Abandoned;
			}
			catch (Exception ex)
			{
				Console.WriteLine();
				Console.WriteLine("Error in sample: {0}", ex.Message);
				// Indicate that the message treatment is not completed.
				ModuleClient moduleClient = (ModuleClient)userContext;
				return MessageResponse.Abandoned;
			}
		}
		    
8.  Save the **Program.cs** file.

##Build and push your module

Now you need to build the solution as a container image and push it to your container registry.

1.  Use the following command to sign in to Docker on your development machine. Use the username, password, and login server from your Azure container registry. You can retrieve these values from the Access keys section of your registry in the Azure portal.

    	docker login -u <ACR username> -p <ACR password> <ACR login server>

2.  In the Visual Studio solution explorer, right-click the project name that you want to build. since you're building a Windows module, the extension should be **Windows.Amd64**.

3.  Select **Build and Push IoT Edge Modules.** The build and push command starts three operations. First, it creates a new folder in the solution called **config** that holds the full deployment manifest, built out of information in the deployment template and other solution files. Second, it runs `docker build` to build the container image based on the appropriate dockerfile for your target architecture. Then, it runs `docker push` to push the image repository to your container registry.

The final image repository looks like **&lt;registry name&gt;.azurecr.io/tempfiltermodule:0.0.1-windows-amd64**.

## Deploy private modules to device

The deployment manifest shares the credentials for your container registry with the IoT Edge runtime. The runtime needs these credentials to pull your private images onto the IoT Edge device. 

1.  Sign in the Azure Portal and Set modules again for your IoT Edge device. 
2. In the **Registry settings** section of the page, provide the credentials to access your private container registries that contain your module images. Use the credentials from the **Access keys** section of your **Azure container registry**.

    ![](./media/m90n-1/container_registry_credential.png)

3.  In the **Deployment modules** section of the page, select **Add** and select **IoT Edge module**.
4. Provide a name for the module, then specify the container image as follow:

    -   Name : **tempFilter**
	-   Image URI :**&lt;registry name&gt;.azurecr.io/tempfiltermodule:0.0.1-windows-amd64**

5.  Tick to set the "Twins Desire Propreites" as:

    	{
      		"properties.desired": {
    			"TemperatureThreshold": 30
      		}
		}

6.  Select **Save**.
7.  Select **Next** to continue to the routes section. Replace the default route as :

    	{
    	  "routes": {
    		"TempFilterModuleToIoTHub": "FROM /messages/modules/tempFilter/outputs/* INTO $upstream",
    		"sensorToTempFilterModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/tempFilter/inputs/input1\")"
    	  }
    	}
    
	Every route needs a source , a sink and an optional condition that you can use to filter messages.see [also](https://docs.microsoft.com/en-us/azure/iot-edge/module-composition)
8.  Review your deployment information, then select **Submit**.

##View your data in the Cloud

Once you apply the deployment manifest to your IoT Edge device, the IoT Edge runtime on the device collects the new deployment information and starts executing on it. Any modules running on the device that aren't included in the deployment manifest are stopped. Any modules missing from the device are started:

	iotedge logs tempFilter -f

   ![](./media/m90n-1/filter-module-log.png)

You can use the [Device Explorer tool](https://github.com/Azure/azure-iot-sdks/releases/download/2016-11-17/SetupDeviceExplorer.msi) to view messages as they arrive at your IoT Hub:

1.  In **Configuaretion** tab, input your [IoT hub owner](https://devblogs.microsoft.com/iotdev/understand-different-connection-strings-in-azure-iot-hub/) conntaction string and click **Update**". 
2.  You can view all IoT device in **Management**.
3.  In the **data** tab, select the device you like to check and click **Monitor** to
View the messages arriving at your IoT Hub. It may take a while for the messages to arrive, because the changes we made to the **TempFilterModule** code wait until the machine temperature reaches 30 degrees before sending messages. It also adds the message type **Alert** to any messages that reach that temperature threshold.

   ![](./media/m90n-1/device-explorer-cloud-message.png)

  
[setup-devbox-Windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md