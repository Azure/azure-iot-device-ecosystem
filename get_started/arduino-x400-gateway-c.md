---
platform: arduino
device: x400 gateway
language: c
---

Run a simple C sample on X400 Gateway device running Arduino
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Tips](#tips)
-   [Next Steps](#NextSteps)


<a name="Introduction"></a>
# Introduction

**About this document**

X400 Gateway embeds two microcontrollers: the first one, an STM32F4, manages the connection with IoT Hub, using the Microsoft IoT library, while the second one, an SAMD21G, runs the user's application.   
Since the connectivity is managed by the STM32, the user just have to write his own application on the SAMD21 microcontroller to interface the device with the real world (sensors, actuators, buses...). The firmware is written using the Arduino IDE and Arduino-like commands. The communication with IoT Hub is managed by using a simple serial API from the Arduino-compatible processor (SAMD21) to the communication processor (STM32F4).   
 
This document describes how to connect X400 with Azure IoT SDK. This multi-step process includes:

-   Register your device with the Iomote service
-   Prepare your computer to write your application
-   Build a sample application for your device to send data on IoT Hub
-   Run a sample C# application to read the messages from the device

<a name="Prerequisites"></a>
# Step 1: Prerequisites
Before starting you need to meet some prerequisites:

-   You need an account for the Iomote service. The Iomote service is based on **IoT Hub**, once you have an Iomote account, your IoT Hub will be set up by the Iomote service for you, together with the service bus, so you'll be ready to communicate with the device. Once you get an X400 device/kit, Iomote will create an account for you.
-  Install the **Arduino IDE** on your computer. You can find many tutorials on the Arduino website  [www.arduino.cc](http://www.arduino.cc) . Once you have installed the IDE you have to add the **software libraries for the X400 gateway**: 
	1. Open the **Preferences** of the Arduino IDE (File-> Preferences)
	2. In the "settings" tab, and in the **Additional board manager URL** field, add this URL `https://github.com/iomote/arduino-ide-bsp/raw/master/package_iomote_index.json`, then click OK.
	3. Open the **Boards Manager** (*Tools -> Board:... -> Board->Board Manager*)
	4. Install the Arduino SAMD Boards (32-bits ARM Cortex-M0+) by Arduino package
	5. Install the IOMote Gateway package
	6. Select X400 device under **IOMote devices** in *Tools -> Board* menu
	7. Install **RTCZero** library by Arduino (*Sketch -> Include Library -> Manage Libraries*)
	8. Compile and upload your sketches as usual

<a name="PrepareDevice"></a>
# Step 2: Register your Device

Once turned on, the device will connect with the Iomote managed IoT hub (no code needed, the connection works out-of-the-box). It can be considered a "landing" IoT Hub, where "orphan" devices connect at their startup, it is used to route the device to the user's IoT Hub. Once the user register the device with his account, the connection string of the new IoT Hub is sent to the device, that will start to communicate with the user's IoT Hub. From now, the user will be able to exchange data with the gateway.
To register the device with your account you have to access your Iomote account, go to the "Devices" section, and click the "Add" button 

<a name="Build"></a>
# Step 3: Build and Run the sample

## 3.1 Running the sample code on the Arduino IDE

Copy the following sketch to the Arduino IDE.

~~~ C++
#include <iomoteClass.h>

#define Serial   SerialUSB 

void setup() 
{
	//	This instruction initializes the board, it's mandatory for any sketch to correctly work with the X400 Cloud Operations!
	Iomote.begin("getting started",0,0,1);	
	
	Serial.write("X400 Getting Started!\r\n");
}

void loop() 
{
	//	Check of front button to detect the push event
	if(Iomote.SwitchRead() == 0)
	{
		//	Two random number are created and are sent to the Iot Hub
		int16_t randomData = rand();
		// 	Adding first number to the JSON string to send 
		int8_t dataResult = Iomote.append((char*)"random data 1", randomData);	
		randomData = rand();
		//	Adding second number to the JSON string
		dataResult += Iomote.append((char*)"random data 2", randomData);
		//	The JSON is finalized adding the timestamp, so the string is ready 
		//	to be sent.
		dataResult += Iomote.object();
		if(dataResult == 0)
		{
			Serial.write("Data added to queue...\r\n");
			//	With the sendData command the data is put in a queue, and wil be 
			//	sent to IoT Hub. 
			int8_t sendResult = Iomote.send();
			if(sendResult == 0)
			{
				Serial.write("Data correctly enqueued and ready to be sent!\r\n");
			}
			else
			{
				Serial.write("Unable to send data now! Result: ");
				Serial.println(sendResult);
			}
		}
		else
		{
			Serial.write("Unable to add data! Result: ");
			Serial.println(dataResult);
		}
		delay(1000);
	}
}
~~~


Upload the sketch on the device using the standard USB connector on the X400 Gateway, then turn on the **serial monitor** on the Arduino IDE to debug the application. 
Once started, the sketch waits for you to push on the X400 front button. When the button is pushed, the gateway will send a couple of random number to the IoT Hub, plus the timestamp. All the data is embedded inside a JSON string, so it'll be easier to parse it on your cloud app. In the next section you'll learn how to read that data from your application.


## 3.2 Reading the message from the device 

### 3.2.1 Node.JS example

For debug purposes we have set up a Node JS application to easily receive the messages sent from the sample application on the device. If you don't have Node JS installed on your computer, you can easily install it from here: [https://nodejs.org](https://nodejs.org).

-   create a folder, for example named *iomote-sb-node-example*
-   inside the folder run the following console commands:
    -  `npm install azure`
-  create a file named ***iomoteSbNode.js*** and copy the following code
~~~ Javascript
// Iomote 
// In this example the service bus connection string and queue names are provided using the 
// service bus connection string provided on your  Iomote Dashboard User Settings
// If you don't want to use command line parameters, just paste your connection string on 
// connStr variable initializzation value instead of using command line arguments!
// 
// For Further reference please refer to Microsoft's documentation
// https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-nodejs-how-to-use-queues
//

var azure = require('azure');

var connStr = process.argv[2]; // <<<=== change this init with '<your_service_bus_connection_string>' if you don't want console params
if (!connStr) 
	throw new Error('Must provide connection string');

var queueName = null;
var entityToken = 'EntityPath=';
var entityIndex = connStr.indexOf(entityToken);
var connStrLen = connStr.length;
if(entityIndex > 0) {
  // set a new queueName value:
  queueName = connStr.slice(entityIndex+entityToken.length, connStrLen);
  var serviceBusConnStr = connStr.substr(0, entityIndex-1);
}
if(!queueName) {
  throw new Error('Must provide connection string');
}
console.log("**********************************************************************");
console.log('* Connecting to ', serviceBusConnStr);
console.log('* Queue name ', queueName);
console.log("**********************************************************************");
var serviceBusService = azure.createServiceBusService(serviceBusConnStr);

// This function is responsable of reading messages from service bus queue
var rxQueue = function() {
  serviceBusService.receiveQueueMessage(queueName, function(error, receivedMessage){
    if(!error) {
      console.log("**********************************************************************");
      //console.log('[message] ', receivedMessage);
      if(receivedMessage.body){
        console.log('[message] ', receivedMessage.body);
      }
      if(receivedMessage.customProperties) {
        let userRoute = receivedMessage.customProperties.user_route;
        let mId = receivedMessage.customProperties.mid;
        let devId = receivedMessage.customProperties['iothub-connection-device-id'];
        if(devId) {
          console.log('[device] ', devId);
        }
        if(mId) {
          console.log('[message id] ', mId);
        }
        if(userRoute) {
          console.log('[user route] ', userRoute);
        }
        console.log("**********************************************************************");
      }
    } 
  });
}
// set timed interval so in case your Service Bus client fails, it will register a new Queue Message received callback
var recInterval = setInterval(rxQueue, 25);

~~~
- on console run the following command
	- `node ./iomoteSbNode.js "YOUR_CONNECTION_STRING_PARAMETER"` (please use the service bus connection string you can find on your account details)


### 3.2.2 C# .NET Core Example

For debug purposes we have C# .NET Core application to easily receive the messages sent from the sample application on the device. If you don't have .NET Core SDK installed on your computer, you can easily install it from here: [Microsoft website](https://www.microsoft.com/net/download/windows).

-   create a folder, for example named *iomote-sb-dotnetcore-example*
-   on console terminal type the following command to create a new Console Application (.NET Core)
	-   `dotnet new console`
-   on console install the needed package to use ServiceBus client
	-   `dotnet add package Microsoft.Azure.ServiceBus`
-   copy the following content to previously created file "Program.cs"

```cs

using System;
using System.Threading.Tasks;
using System.Threading;
using System.Text;
using Microsoft.Azure.ServiceBus;

namespace iomote_sb_dotnetcore_example
{
    class Program
    {
        // Parse connection string parameters to get queue name and connection string values...
        static string iomoteSbConnectionString = "<PUT HERE THE SERVICE BUS CONNECTION STRING YOU CAN FIND ON IOMOTE DASHBOARD>";
        static string ServiceBusConnectionString = null;
        static string QueueName = null;
        static IQueueClient queueClient;

        static void Main(string[] args)
        {
            Console.WriteLine("********************************************************************\r\nIomote Service Bus C# Example\r\n********************************************************************");
            
            int queuePos = iomoteSbConnectionString.IndexOf(";EntityPath=");
            QueueName = iomoteSbConnectionString.Substring(queuePos+12);
            ServiceBusConnectionString = iomoteSbConnectionString.Substring(0, queuePos);

            Console.WriteLine("Connection string: " + ServiceBusConnectionString);
            Console.WriteLine("Queue Name: " + QueueName);
            Console.WriteLine("********************************************************************");
            // https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient
            MainAsync(args).GetAwaiter().GetResult();
        }

        static async Task ProcessMessagesAsync(Message message, CancellationToken token)
        {
            // Process the message
            object deviceSender = null;
            object body = null;

            Console.WriteLine("********************************************************************");
            if(message.UserProperties.TryGetValue("iothub-connection-device-id", out deviceSender))
            {
                Console.WriteLine($"Received message from {deviceSender.ToString()}");
            }
            if (message.Body != null)
            {
                body = Encoding.UTF8.GetString(message.Body);
                Console.WriteLine($"Message application Data: {body.ToString()}");
            }
            Console.WriteLine("********************************************************************");
            // Complete the message so that it is not received again.
            // This can be done only if the queueClient is opened in ReceiveMode.PeekLock mode (which is default).
            await queueClient.CompleteAsync(message.SystemProperties.LockToken);
        }

        // Use this Handler to look at the exceptions received on the MessagePump
        static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
        {
            Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
            return Task.CompletedTask;
        }

        static void RegisterOnMessageHandlerAndReceiveMessages()
        {
            // Configure the MessageHandler Options in terms of exception handling, number of concurrent messages to deliver etc.
            var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
            {
                // Maximum number of Concurrent calls to the callback `ProcessMessagesAsync`, set to 1 for simplicity.
                // Set it according to how many messages the application wants to process in parallel.
                MaxConcurrentCalls = 4,

                // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
                // False value below indicates the Complete will be handled by the User Callback as seen in `ProcessMessagesAsync`.
                AutoComplete = false
            };

            // Register the function that will process messages
            queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
        }

        static async Task MainAsync(string[] args)
        {
            queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

            // Register QueueClient's MessageHandler and receive messages in a loop
            RegisterOnMessageHandlerAndReceiveMessages();
            
            Console.WriteLine("********************************************************************\r\nPress any key to exit after receiving all the messages\r\n********************************************************************\r\n");
            Console.ReadKey();

            await queueClient.CloseAsync();
        }
    }
}


```
-   Change the value of **iomoteSbConnectionString** variable with your service bus connection string 
-   Run the command
	- `dotnet restore`
-   Run the application typing the command
	- `dotnet run`
 

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
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md