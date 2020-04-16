# Getting started - running a Java service sample application

This "Getting Started" document shows you how to build and run any of the samples contained within the Java SDLK service samples folder. 

## Prerequisites

Before you get started, you should:
- [Prepare your development environment][devbox-setup]
- [Setup your IoT Hub][setup-iothub]
- [Register your device in IoT Hub][register-device]

## Build and run the samples

You can build and run the samples using Maven.

To build and run the Device Manager Sample application:

1. Preparing the Device Manager Sample application:

	Navigate to the main sample file for Device Manager
	It can be found at: 
	{IoT device SDK root}\java\service\samples\device-manager-sample\src\main\java\samples\com\microsoft\azure\iot\service\sdk\SampleUtils.java

	Locate the following code in the file:

	```
	public static final String iotHubConnectionString = "[sample iot hub connection string goes here]";
    public static final String storageConnectionString = "[sample storage connection string goes here]";
    public static final String deviceId = "[Device name goes here]";
    public static final String exportFileLocation = "[Insert local folder here - something like C:\\foldername\\]";
	```
	
    Replace "[sample iot hub connection string goes here]" with the connection information used in Device Explorer.
	Replace "[Device name goes here]" with the name of the device you want to create, read. update or delete.
    
    Note: The **storageConnectionString** and **exportFileLocation** values are only used by the import and export samples.

	Locate the main function in the DeviceManagerSample.java file:
    
    ```
    public static void main(String[] args) throws IOException, URISyntaxException, Exception
    ```
    
	Notice there are function calls implemented for each CRUD operation and they called from the main function in order.

	Pick the operations you want to run, and comment out the others. Notice that the add operation requires you to provide some keys to associate with the device.
    
2. Building the Device Manager Sample application:

    To build the Device Manager Sample application using Maven, at a command prompt navigate to the **\java\service\samples\device-manager-sample** folder. Then execute the following command and check for build errors:
    
    ```
    mvn clean package
    ```

3. Running the Device Manager Sample application:

	To run the Device Manager Sample application using Maven, execute the following command.
    
    ```
    mvn exec:java -Dexec.mainClass="samples.com.microsoft.azure.iot.service.sdk.DeviceManagerSample"
    ```

	You can verify the result of your operation by using [Device Explorer or iothub-explorer tool][register-device].

To build and run the Service Client Sample application:

1. Preparing the Service Client Sample application:

	Navigate to the main sample file for Service Client.
	It can be found at: 
	{IoT device SDK root}\java\service\samples\service-client-sample\src\main\java\samples\com\microsoft\azure\iot\service\sdk\ServiceClientSample.java

	Locate the following code in the file:

	```
	private static final String connectionString = "[Connection string goes here]";
	private static final String deviceId = "[Device name goes here]";
	```
    
	Replace "[Connection string goes here]" with the connection information used in Device Explorer.
	Replace "[Device name goes here]" with the name of the device you are using.

	Locate the main function:
    
    ```
	public static void main(String[] args) throws IOException, URISyntaxException, Exception
    ```

	Update the value of the local variable "commandMessage" to contain the message you want to send to the device.
    
2. Building the Service Client Sample application:

    To build the Service Client Sample application using Maven, at a command prompt navigate to the **\java\service\samples\service-client-sample** folder. Then execute the following command and check for build errors:
    
    ```
    mvn clean package
    ```

3. Running the Service Client Sample application:

	To run the Service Client Sample application using Maven, execute the following command.
    
    ```
    mvn exec:java -Dexec.mainClass="samples.com.microsoft.azure.iot.service.sdk.ServiceClientSample"
    ```

	You can verify the result of your operation by using [Device Explorer or iothub-explorer tool][register-device].

## Documentation

The documentation can be found [here](https://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html).

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
[devbox-setup]: java-devbox-setup.md
[setup-iothub]: ../setup_iothub.md
[register-device]: ../manage_iot_hub.md

