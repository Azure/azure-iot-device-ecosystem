How to certify IoT devices running Windows 10 with Azure IoT SDK 
===
---


# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Sign Up To Azure IoT Hub](#Step_1:_Sign_Up)
-   [Step 2: Register Device](#Step_2:_Register)
-   [Step 3: Build and Validate the Sample using C# Client Libraries](#Step_3:_Build_and_Validate)
    -   [3.1 Prepare your development environment](#Step_3_1:_Development)
    -   [3.2 Build the Samples](#Step_3_2:_Build)
    -   [3.3 Run and Validate the Samples](#Step_3_3:_Run)
-   [Step 4: Package and Share](#Step_4:_Package_Share)
    -   [4.1 Package build logs and sample test results](#Step_4_1:_Package)
    -   [4.2 Share package with Engineering Support](#Step_4_2:_Share)
    -   [4.3 Next steps](#Step_4_3:_Next)
-   [Step 5: Troubleshooting](#Step_5:_Troubleshooting)

<a name="Introduction"></a>
# Introduction

**About this document**

This document provides step by step guidance to IoT hardware publishers on how to certify an IoT enabled hardware with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub 
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device
-   Packaging and sharing the logs  

**Prepare**

Before executing any of the steps below, read through each process, step
by step to ensure end to end understanding.

You should have the following items ready before beginning the process:

-   Computer with GitHub installed and access to the [azure-iot-sdk-csharp](https://github.com/Azure-Samples/azure-iot-samples-csharp) GitHub public repository.
-   Install Visual Studio 2015 and Tools. You can install any edition of Visual Studio, including the free Community edition.

<a name="Step_1:_Sign_Up"></a>
# Step 1: Sign Up To Azure IoT Hub

Follow the instructions [here](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-csharp-csharp-getstarted#create-an-iot-hub) on how to sign up to the Azure IoT Hub service.

As part of the sign up process, you will receive the connection string.

-   **IoT Hub Connection String**: An example of IoT Hub Connection String is as below:

        HostName=[YourIoTHubName];SharedAccessKeyName=[YourAccessKeyName];SharedAccessKey=[YourAccessKey]

<a name="Step_2:_Register"></a>
# Step 2: Register Device

In this section, you will register your device using DeviceExplorer. The DeviceExplorer is a Windows application that interfaces with Azure IoT Hub and can perform the following operations:

-   Device management
    -   Create new devices
    -   List existing devices and expose device properties stored on Device Hub
    -   Provides ability to update device keys
    -   Provides ability to delete a device
-   Monitoring events from your device
-   Sending messages to your device

To run DeviceExplorer tool, use following configuration string as described in [Step1](#Step_1:_Sign_Up):

-   IoT Hub Connection String

**Steps:**

1.  Click [here](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/readme.md) to download and install DeviceExplorer.

2.  Add connection information under the **Configuration** tab and click the **Update** button.

3.  Create and register the device with your IoT Hub using instructions as below.

    a. Click the **Management** tab.    
    
    b. Your registered devices will be visible in the list. In case your device is not there in the list, click **Refresh** button. If this is your first time, then you shouldn't retrieve anything.
       
    c. Click **Create** button to create a device ID and key. 
    
    d. Once created successfully, device will be listed in DeviceExplorer. 
    
    e. Right click the device and from context menu select "**Copy connection string for selected device**".
    
    f. Save this information in Notepad. You will need this information in later steps.

***Not running Windows on your PC?*** - Please follow the instructions [here](<https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md>) to provision your device and get its credentials.

<a name="Step_3:_Build_and_Validate"></a>
# Step 3: Build and Validate the Sample using C# Client Libraries 

This section walks you through building, deploying and validating the IoT Client SDK on your device running Windows 10 operating system. You will install the necessary prerequisites on your device. Once done, you will build and deploy the IoT Client SDK, and validate the sample tests required for IoT certification with the Azure IoT SDK.

<a name="Step_3_1:_Development"></a>
## 3.1 Prepare your development environment

- Install the latest .NET Core from https://dot.net
- Install .NET Framework 4.7 Developer Pack: https://support.microsoft.com/en-us/help/3186612/the-net-framework-4-7-developer-pack-and-language-packs
- Install .NET Framework 4.5.1 Developer Pack: https://www.microsoft.com/en-us/download/details.aspx?id=40772
- As admin (one-time setup):
    Enable Powershell script execution on your system. See http://go.microsoft.com/fwlink/?LinkID=135170 for more information.
    `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

<a name="Step_3_2:_Build"></a>
## 3.2  Build the Samples

1.  Open a device console (command prompt or a powershell window) and change to your local SDK **azure-iot-samples-csharp-master** directory.

2.  Add the Iot Hub device connection string on your device as an environment variable:

		setx IOTHUB_DEVICE_CONN_STRING <yourDeviceConnectionString>

3.  Run the following command to build the SDK:

		build.cmd -config Release

<a name="Step_3_3:_Run"></a>
## 3.3 Run and Validate the Samples
    
In this section you will run the Azure IoT client SDK samples to validate the communication between your device and Azure IoT Hub. You will send the messages to the Azure IoT Hub service and validate that IoT Hub has successfully receive the data. You will also monitor any messages sent from the Azure IoT Hub to client.

***Note:*** *Take screenshots of all the operations you will perform in this
section. These will be needed in [Step 4](#Step_4_2:_Share).*

### 3.3.1 Send Device Events to IoT Hub

1.  Launch the DeviceExplorer as explained in [Step 2](#Step_2:_Register) and navigate to **Data** tab. Select the device name you created from the drop-down list of device IDs and click **Monitor** button.

    ![DeviceExplorer\_Monitor](images/3_3_1_01.png)

2.  DeviceExplorer is now monitoring data sent from the selected device to the IoT Hub.
     

3.  From the device console, run the sample using following command:

		cd iot-hub\Samples\device\MessageSample\bin\Release\netcoreapp2.1
		dotnet MessageSample.dll
        
**To run for the different protocols:**	

-   Follow the below commands on device.

        cd iot-hub\Samples\device\MessageSample
        notepad Program.cs

-   This launches a text editor. Scroll down to the
    protocol information.
    
-   Find the below code:

        private static TransportType s_transportType = TransportType.Amqp;
	
    The default protocol used is AMQP. Code for other protocols(HTTP/MQTT) are mentioned just below the above line in the script.
    Comment/Uncomment the line as per the protocol you want to use and Save the changes by pressing Ctrl+S.
    
    We need to build the app to apply the changes. For that follow the steps again from [Step 3.2](#Step_3_2:_Build).

***Note:*** *Ignore if you face any build issues related to the “FileUploadSample” section and continue the steps.*
   
4. You should be able to see the events received in device console on successful execution.

	**If HTTP protocol:**

	![DeviceExplorer\_Notification\_Send](images/terminal_message_send_from_device_http.png)

	**If MQTT protocol:**

	![DeviceExplorer\_Notification\_Send](images/terminal_message_send_from_device_mqtt.png)

	**If AMQP protocol:**

	![DeviceExplorer\_Notification\_Send](images/terminal_message_send_from_device_amqp.png)

5. You should be able to see the events received in the DeviceExplorer's data tab.

	**If HTTP protocol:**	

     ![DeviceExplorer\_Notification\_Send](images/device_message_send_from_device_http.png)

	**If MQTT protocol:**

	 ![DeviceExplorer\_Notification\_Send](images/device_message_send_from_device_mqtt.png)

	**If AMQP protocol:**
	
	 ![DeviceExplorer\_Notification\_Send](images/device_message_send_from_device_amqp.png)

### 3.3.2 Receive messages from IoT Hub

1.  To verify that you can send messages from the IoT Hub to your device, go to the **Messages to Device** tab in DeviceExplorer.

2.  Select the device you created using Device ID drop down.

3.  Add some text to the Message field, then click Send.

   	![DeviceExplorer\_Notification\_Send](images/device_message_receive_from_device_http.png)

4. You should be able to see the message received in the device console window.
	
	**If HTTP protocol:**

	![DeviceExplorer\_Notification\_Send](images/terminal_message_receive_from_device_http.png)

	**If MQTT protocol:**

	![DeviceExplorer\_Notification\_Send](images/terminal_message_receive_from_device_mqtt.png)

	**If AMQP protocol:**

	![DeviceExplorer\_Notification\_Send](images/terminal_message_receive_from_device_amqp.png)
    
<a name="Step_4:_Package_Share"></a>
# Step 4: Package and Share

<a name="Step_4_1:_Package"></a>
## 4.1 Package build logs and sample test results
  
Package the following artifacts from your device:

1.  Build logs from section 3.2.
2.  All the screenshots that are shown above in "**Send Device Events to IoT Hub**" section.
3.  All the screenshots that are shown above in "**Receive messages from IoT Hub**" section.
4.  Send us clear instructions of how to run this sample with your hardware
    (explicitly highlighting the new steps for customers). Please use the template available [here](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/iotcertification/templates/template-windows-csharp.md) to create your device-specific instructions.

    As a guideline on how the instructions should look please refer the
    examples published on github repository [here](https://github.com/Azure/azure-iot-device-ecosystem/tree/master/get_started).

<a name="Step_4_2:_Share"></a>
## 4.2 Share package with the Azure IoT Certification Team

1.  Go to [Partner Dashboard](<https://catalog.azureiotsuite.com/devices>).
2.  Click on Upload icon at top-right corner of your device.

    ![Share\_Results\_upload\_icon](images/4_2_01.png)

3.  This will open an upload dialog. Browse your file(s) by clicking **Upload** button.

    ![Share\_Results\_upload\_dialog](images/4_2_02.png)

    You can upload multiple files for same device.

4.  Once you have uploaded all the files, click on **Submit for Review** button.

    ***Note:*** *Please contact iotcert team to change/remove the files once you submit them for review.*
 

<a name="Step_4_3:_Next"></a>
## 4.3 Next steps

Once you shared the documents with us, we will contact you in the following 48 to 72 business hours with next steps.

<a name="Step_5:_Troubleshooting"></a>
# Step 5: Troubleshooting

Please contact engineering support on <iotcert@microsoft.com> for help with  troubleshooting.
