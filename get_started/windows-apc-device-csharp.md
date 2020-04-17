---
platform: windows 10 
device: b&r apc
language: csharp
---

Run a simple C# sample on B&R APC device running Windows 10
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Sign Up To Azure IoT Hub](#Step_1:_Sign_Up)
-   [Step 2: Register Device](#Step_2:_Register)
-   [Step 3: Build and Validate the Sample using C# Client Libraries](#Step_3:_Build_and_Validate)
    -   [3.1 Connect the Device](#Step_3_1:_Connect)
    -   [3.2 Build the Samples](#Step_3_2:_Build)
    -   [3.3 Run and Validate the Samples](#Step_3_3:_Run)
-   [Step 4: Troubleshooting](#Step_4:_Troubleshooting)

<a name="Introduction"></a>
# Introduction

**About this document**

This multi-step process includes:
-   Configuring Azure IoT Hub 
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device
-   Packaging and sharing the logs  

**Prepare**

Before executing any of the steps below, read through each process, step
by step to ensure end to end understanding.

You should have the following items ready before beginning the process:

-   Computer with GitHub installed and access to the [azure-iot-sdk-csharp](https://github.com/Azure/azure-iot-sdk-csharp) GitHub public repository.
-   Install Visual Studio 2015 and Tools. You can install any edition of Visual Studio, including the free Community edition.

    Make sure to select the "Universal Windows App Development Tools", the component required for writing apps Windows 10:
    
    ![Install\_required\_tools](./media/APCimages/vs_install_tools_for_windows10.png)
-   Required hardware running Windows 10 IoT Core to certify

     ***Note:*** *If you need assistance installing Windows 10 IoT Core , please visit <https://www.windowsondevices.com> or contact us at <iotcert@microsoft.com>*

<a name="Step_1:_Sign_Up"></a>
# Step 1: Sign Up To Azure IoT Hub

Follow the instructions [here](https://account.windowsazure.com/signup?offer=ms-azr-0044p) on how to sign up to the Azure IoT Hub service.

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

1.  Click [here](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) to download and install DeviceExplorer.

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

This section walks you through building, deploying and validating the IoT Client SDK on your device running Windows 10 IoT Core operating system. You will install the necessary prerequisites on your device. Once done, you will build and deploy the IoT Client SDK, and validate the sample tests required for IoT certification with the Azure IoT SDK.

<a name="Step_3_1:_Connect"></a>
## 3.1 Connect the Device

1.  Connect the board to your network using an Ethernet cable. This step
    is required, as the sample depends on internet access.

2.  Plug the device into your computer using a micro-USB cable.

<a name="Step_3_2:_Build"></a>
## 3.2  Build the Samples

1. Clone [Azure IoT SDK](https://github.com/Azure/azure-iot-sdk-csharp.git) repository to your machine.

2.  Start a new instance of Visual Studio 2015. Open the **iothub_csharp_deviceclient.sln** solution (/azure-iot-sdk-csharp/device) from your local copy of the repository.

3.  In Visual Studio, from **Solution Explorer**, navigate to the **UWPSample(Universal Windows)** project.

4.  Locate the following code in the **ConnectionStrings.cs** file:

        public const string DeviceConnectionString = "<replace>";

5.  Replace `<replace>` with the connection string for your device and **Save** the changes. You can get the connection string from DeviceExplorer as explained in [Step 2](#Step_2:_Register).

6.  Choose the right architecture (x86 or ARM, depending on your device) and set the debugging method to "Remote Machine":

    ![VisualStudio\_Select\_Architecture](./media/APCimages/vs_select_arch.png)
    
7.  To deploy the binaries on your device, right-click on the UWPSample project in the **Solution Explorer**, select **Properties** and navigate to the **Debug** tab:

    ![VisualStudio\_Properties\_Debug](./media/APCimages/vs_properties_debug.png)

    Type in the name of your device. Make sure the "Use authentication" box is unchecked.

8.  Build the solution.

<a name="Step_3_3:_Run"></a>
## 3.3 Run and Validate the Samples
    
In this section you will run the Azure IoT client SDK samples to validate the
communication between your device and Azure IoT Hub. You will send the messages to the Azure IoT Hub service and validate that IoT Hub has successfully receive the data. You will also monitor any messages sent from the Azure IoT Hub to client.


### 3.3.1 Send Device Events to IoT Hub

1.  Launch the DeviceExplorer as explained in [Step 2](#Step_2:_Register) and navigate to **Data** tab. Select the device name you created from the drop-down list of device IDs and click **Monitor** button.

    ![DeviceExplorer\_Monitor](./media/APCimages/device_explorer_monitor.png)
    
2.  DeviceExplorer is now monitoring data sent from the selected device to the IoT Hub.
     
3.  In Visual Studio, from **Solution Explorer**, right-click the **UWPSample(Universal Windows)** project, click **Debug &minus;&gt; Start new instance** to build and run the sample. 
      
4. You should be able to see the events received in the DeviceExplorer's data tab.

    ![DeviceExplorer\_Events\_Received](./media/APCimages/3_3_windows_cs_send.png)

### 3.3.2 Receive messages from IoT Hub

1.  To verify that you can send messages from the IoT Hub to your device, go to the **Messages to Device** tab in DeviceExplorer.

2.  Select the device you created using Device ID drop down.

3.  Add some text to the Message field, then click Send.

    ![DeviceExplorer\_Notification\_Send](./media/APCimages/3_3_windows_cs_rec.png)

4.  You should be able to see the message received in the device console window.
 
<a name="Step_4:_Troubleshooting"></a>
# Step 4: Troubleshooting

Please contact engineering support on <support@br-automation.com> for help with troubleshooting.

Reference: Some information in this document is obtained from [here.](https://github.com/Azure/azure-iot-device-ecosystem/tree/master/iotcertification)
