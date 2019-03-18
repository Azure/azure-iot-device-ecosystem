---
platform: edge debian linux
device: openblocks vx2
language: c
---

Run a simple C sample on OpenBlocks VX2 device running Debian Linux
===
---

# Table of Contents

-   [Install instruction of IoT Edge runtime package and other.](#edge)
-   [Set-up instruction for IoT Edge.](#Setup)
-   [Make dummy module with Visual Code Studio.](#Dummy)

<a name="edge"></a>
# Step 1: Install instruction of IoT Edge runtime package and other.

The service softwares of OpenBlocks IoT Family are provided in a Debian style package and can be installed using the [Maintenance]-[Enhancements] tab in WEB UI.

## 1.1 Advance preparation

Before installing IoT Edge runtime and Moby, perform initial settings such as network, accroding to chapter 2 and 3 of the **OpenBlocks IoT Family WEB UI Set-up Guide Ver.3.2.0**.

## 1.2 Install instruction of IoT Edge runtime package.

  a.  Click the [Maintenance]-[Enhancements] tab.

  b.  Choose the **Azure IoT Edge** in pull-down menu of **Function to add** item.

  c.  Click the **Execution** button.

-   When click the **Execution** button, the **Status check** button will be displayed.
   
-   Installation of the package takes time, Please click the **Status check** button and check the progress of installation.

## 1.3 Install instruction of Moby package.

  a.  Click the [Maintenance]-[Enhancements] tab.

  b.  Choose the **Moby** in pull-down menu of "Function to add" fields.

  c.  Click the **Execution** button.

## 1.4 Install instruction of IoT data control package.

The IoT data control package includes packages that communicate with IoT Hub or IoT Edge called PD repeater,
handler packages that read data from BLE devices and Modbus devices, and so on.

  a.  Click the [Maintenance]-[Enhancements] tab.

  b.  Choose the **IoT dada control** in pull-down menu of "Function to add" fields.

  c.  Click the **Execution** button.

## 1.5 Rebooting

After completing installation the unit will require rebooting.

  a.  Click the [Maintenance]-[Shutdown/Reboot] tab.

  b.  Click the **Run** button in **Reboot** menu.

>**Caution**

>Package of PD repeater which can be downloaded at present,It contains the problem reported below at this moment.

>  https://github.com/Azure/iotedge/pull/76
   
>Also, a problem has been found in the preinstalled PD Agent for IoT Hub.

>Connect with the serial console (see 6-6-1) and install following Debian package witch included in the result package with dpkg command.

>  pd-agent-iothub_1.0.0-0dev-plathome_amd64.deb
>  pd-repeater_1.1.0-0dev-plathome_amd64.deb


<a name="Setup"></a>
# Step 2: Set-up instruction for IoT Edge

    Edit /etc/iotedge/config.yaml via WEB UI.

  a. Click the [Service] tab to to display the link to the installed service software.

   If [Service] tab not displayed, click the [Dashboard] tab and then.

  b. Click the link of **Azure IoT Edge** to display the [Azure IoT Edge] tab.

  c. Click the [Azure IoT Edge]-[Setup]

  d. Edit the **IoT Edge Gateway hostname** fields if necessary.

  e. Fill in **Connection Strings** fields with connection strings of IoT Edge device.

  f. To set the certificate, set the radio button in the "Certificate setting" field to 

   **Enable** and set the path name in the **Device certificate**,**Device private key** and "Root CA" fields, if necessary.

   Certificates files can be uploaded using the [System]-[File Management] tab.

  g. Click the **Run** button to re-write `/etc/iotedge/config.yaml` and restart IoT Edge runtime.

  -   Using [Extentions]-[Command exec.] Tab, by executing **cat /etc/iotedge/config.yaml**. You can check the setting.
  -   It is also possible to edit `/etc/iotedge/config.yaml` with the vi command from the serial console using a terminal application such as TeraTerm.
  -   To restart the IoT Edge runtime, click the "reboot" button on the [Azure IoT Edge]-[Edge status] tab, or type **/etc/rc.init/iotedge restart** in the serial console.
  -   For connection to the serial console, please refer to section 2-6 of of the **OpenBlocks IoT Family Developers Guid ver.3.1.0**.
  -   See also the **OpenBlocks IoT Family Azure IoT Edge Set-up Guid ver.3.2.0**.
  
<a name="Dummy"></a>
# Step 3: Make dummy module with Visual Code Studio

The PD Repeater can send data to the $edgehub process of IoT Edge using the MQTT protocol.

IoT Edge side does not need any special handler, just prepare a dummy module that defines the name of the socket.

Create and deploy dummy module using Visual Code Studio, according to following tutorial.

    https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-csharp-module

The daeme module is a module that does not do anything. Of the Init () function of Program.cs, the processing related to connection and callback is commented out as follows.
(This Program.cs is included in the result package.)

    static async Task Init()
        {
            MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
            ITransportSettings[] settings = { mqttSetting };

            // Open a connection to the Edge runtime
            ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
            //await ioTHubModuleClient.OpenAsync();
            //Console.WriteLine("IoT Hub module client initialized.");

            // Register callback to be called when a message is received by the module
            //await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", PipeMessage, ioTHubModuleClient);
        }

The name of the module is **dummyModule** for the explanation.

It also need to modify deployment.json file as fllows.
(This json file is included in the result package.)

   1. Remove **tempSensor** in **modules** property.

   2. Modify **routes** property. as fllows.

        "routes": {
                "dummyModuleToIoTHub": "FROM /* INTO $upstream"
         },
