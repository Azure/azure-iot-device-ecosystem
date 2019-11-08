# Set up and manage Azure IoT Hub

Azure IoT Hub is a fully managed service that enables reliable and secure bi-directional communications between millions of IoT devices and an application back end. You can learn more about Azure IoT Hub visiting the [documentation site][iothub-landing].

Before you can communicate with IoT Hub from a device you must **create an IoT hub instance** in your Azure subscription and then **provision your device in your IoT hub**.

Because of developers preferences and constrains, there are several ways you can create an instance of Azure IoT Hub service and manage this instance. Below are links to resources that will walk you through the steps required to setup an IoT hub and manage it.

## Create an Azure IoT hub... 
* ... [using the Azure portal]
* ... [using the Azure CLI 2.0]  - Python command line
* ... [using the Azure CLI 1.0]  - Node.js command line
* ... [using PowerShell and a resource manager template]
* ... [using C# and a resource manager template]
* ... [using C# and the resource provider REST APIs]


## Manage an Azure IoT hub
Once you have an Azure IoT hub instance deployed, you will need to manage and interact with it to perform the following operations:
* Work with the device registry (Create, Update, Delete device IDs)
* Retrieve device credentials
* Retrieve user credentials
* Send Cloud to Device messages to devices
* Work with Device Twins
* Invoke Device Direct Methods
* Monitor operations of the service

There is some of this interaction that can happen through the [Azure portal], but most of the interaction will happen through tools and leveraging the various service client SDKs.

### Retrieving user credentials to interact with the service (not as a device!)
The first thing  you will need to do before you can use a tool or start developing an application that will interact with the IoT hub using one of the service client SDKs is to retrieve user credentials.

> It is important to understand the difference between user credentials and device credentials:
> * The device credentials are managed by the [IoT Hub identity registry](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-identity-registry) and are to be used by code on **devices**
> * The user credentials are set at the IoT Hub settings level and allow to define user access policies for applications that will **manage** the IoT hub.
> Details on Control access to IoT Hub can be found [here](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-security).

You will need to get user credentials with the right permissions to interact with the identity registry, to send C2D messages, and to work with the Device Twins and Methods.
The user credentials can easily be found on the [Azure portal] in the "Shared Access Policies" section of the [IoT hub settings blade](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-through-portal#change-the-settings-of-the-iot-hub).
Now you have the credentials, you can create 

### Create new device in the IoT Hub device identity registry...
* ... using [azure-iot-cli-extension]
* ... using [Device Explorer]  (Windows desktop application)
* ... using the service client SDK ...
  * ... [for C#]
  * ... [for Node.js]
  * ... [for java]
  * ... [for Python]
* ...  [using Azure CLI v2.0]  (Python command line tool)
  
### Monitor IoT Hub operations
There is a way to monitor an Azure IoT hub operations as all of these are logged into an Event Hub. This can help debug applications developed to interact and manage an IoT hub.
Everything you need to know about IoT Hub operations monitoring is [here][azure iot operations monitoring].

### Work and interact with Devices using the tools and SDKs
Once you have a device ID, you can provision your device with it and start interacting with it from the Cloud through IoT Hub.

When developing, you can leverage some tools that will make your life easier. The below will allow you to do pretty much everything you need to do when developing and testing IoT devices connecting to Azure IoT:
* [iothub-explorer]  (node.js command line tool)
* [Device Explorer]  (Windows desktop application)

To build applications to manage the IoT hub and interact with devices from the Cloud, you can leverage one of our service client SDKs:
* [Azure IoT service client SDK for C#]
* [Azure IoT service client SDK for Node.js]
* [Azure IoT service client SDK for Java]
* [Azure IoT service client SDK for Python]


[iothub-landing]: https://docs.microsoft.com/azure/iot-hub
[Azure portal]: https://portal.azure.com
[using the Azure portal]: https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal
[using the Azure CLI 2.0]: https://docs.microsoft.com/azure/iot-hub/iot-hub-create-using-cli
[using the Azure CLI 1.0]: https://docs.microsoft.com/azure/iot-hub/iot-hub-create-using-cli-nodejs
[using C# and a resource manager template]: https://docs.microsoft.com/azure/iot-hub/iot-hub-rm-template
[using PowerShell and a resource manager template]: https://docs.microsoft.com/azure/iot-hub/iot-hub-rm-template-powershell
[using C# and the resource provider REST APIs]: https://docs.microsoft.com/azure/iot-hub/iot-hub-rm-rest
[azure-portal]: https://portal.azure.com
[azure iot operations monitoring]: https://docs.microsoft.com/azure/iot-hub/iot-hub-operations-monitoring
[azure-iot-cli-extension]: https://github.com/azure/azure-iot-cli-extension
[Device Explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[for C#]: https://docs.microsoft.com/azure/iot-hub/iot-hub-csharp-csharp-getstarted#create-a-device-identity
[for Node.js]: https://docs.microsoft.com/azure/iot-hub/iot-hub-node-node-getstarted#create-a-device-identity
[for java]: https://docs.microsoft.com/azure/iot-hub/iot-hub-java-java-getstarted#create-a-device-identity
[for Python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/service/samples
[using Azure CLI v2.0]: https://docs.microsoft.com/cli/azure/iot/device#create
[Azure IoT service client SDK for C#]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[Azure IoT service client SDK for Node.js]: https://github.com/azure/azure-iot-sdk-node/tree/master/service
[Azure IoT service client SDK for Java]: https://github.com/azure/azure-iot-sdk-java/tree/master/service
[Azure IoT service client SDK for Python]: https://github.com/azure/azure-iot-sdk-python/tree/master/service
