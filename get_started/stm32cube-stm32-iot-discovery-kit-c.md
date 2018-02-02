---
platform: STM32 Cube 
device: STM32L4 Discovery kit IoT node (B-L475E-IOT01A)
language: c

---

Run a sample application on STM32L4 Discovery kit IoT node (B-L475E-IOT01A)
===
---

# Table of Contents
-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect [STM32L4 Discovery Kit][lnk-discovery] to the Microsoft Azure IoT Hub, by leveraging on Azure IoT Device SDK. 
This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering [STM32L4 Discovery Kit][lnk-discovery] to Azure IoT Hub
-   Deploy Azure IoT SDK on [STM32L4 Discovery Kit][lnk-discovery] through pre-compiled binaries to implement data telemetry and device management sample applications
 

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process.

## 1.1 Development environment
- For WindowsOS, the [STM32 ST-Link Utility][lnk-stlink] (requires registration to my.st.com)
- A serial terminal installed in your PC (e.g. [TeraTerm][lnk-teraterm] for Windows) 
- [Setup your IoT hub][lnk-setup-iot-hub]
- [DeviceExplorer][lnk-dev-exp] (Windows OS only) or [iothub-explorer][lnk-iot-exp] ([iothub-explorer][lnk-iot-exp] is used as reference in this guide, requires Node.js version 4.x or newer)
- [Provision your device and get its credentials][lnk-manage-iot-hub]


## 1.2 Hardware components
The [STM32L4 Discovery Kit][lnk-discovery] by STMicroelectronics enables a wide diversity of applications by exploiting multilink communication (Wi-Fi, BLE, Sub-GHz), 
multiway sensing (presence detection, inertial and environmental) together with ARM® Cortex®-M4 core-based STM32L4 Series features.
 
![][1]
 

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
To start using [STM32L4 Discovery Kit][lnk-discovery] simply connect the board to your PC using a micro USB cable. Verify you are connecting the cable to socket 
marked as ```USB STLink```. Once connected to your PC, the board will appear as a storage device.

![][2]


<a name="Build"></a>
# Step 3: Run the sample 


<a name="Load"></a>
### 3.1 Download firmware and use pre-compiled binary

1. Download [FP-CLD-AZURE1][lnk-fp-cld-azure] Function Pack. The Function Pack contains all the required drivers to use the [STM32L4 Discovery Kit][lnk-discovery] board with Wi-Fi expansion boards, together with pre-integrated Microsoft Azure IoT SDK. 
2. Unzip the package and look for the pre-compiled binary named ```Projects\Multi\Applications\Azure_Sns_DM\Binaries\B-L475E-IOT01\Azure_Sns_DM_BL.bin```. 
3. Flash the microcontroller by dragging the binary as shown in picture below 
 
![][3]


### 3.2 Connect and send messages to Azure IoT Hub 

To visualize log messages from [STM32L4 Discovery Kit][lnk-discovery] board, configure your serial terminal (e.g. [TeraTerm][lnk-teraterm] for Windows) with the following parameters 
- BaudRate : 115200
- Data : 8 bit
- Parity : none
- Stop : 1 bit 
- Flow Control : none
- Local Echo : enabled 

Press ```RESET``` button onboard [STM32L4 Discovery Kit][lnk-discovery] to restart the application; when asked, enter the Wi-Fi credentials (default SSID:"STM", PWD:"STMdemoPWD"). 

![][4]


Paste the device connection string when requested.
 
![][5]

Once connected to the IoT Hub, the application transmits periodically messages containing inertial and environmental data read 
from sensors onboard [STM32L4 Discovery Kit][lnk-discovery].

To visualize messages received in IoT Hub with iothub-explorer, open Node.js command prompt and insert the 
following commands:
```
iothub-explorer login <iot-hub-connection-string>
iothub-explorer monitor-events <device name> --login <iot-hub-connection-string>
```
The application can be stopped by pressing blue ```USER``` button.

### 3.3 Receive messages from IoT Hub

See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages from IoT Hub.
Messages received by STM32 [STM32L4 Discovery Kit][lnk-discovery] are printed over serial terminal interface once received. 
The following cloud-to-device messages are interpreted by the application: 
- Pause : pause the application (message to be entered in the form ```{"Name":"Pause", "Parameters":{}}``` )
- Play : restart the application after a pause (message to be entered in the form ```{"Name":"Play", "Parameters":{}}``` )
- LedOn/LedOff : turn on/off LED2 onboard Nucleo (message to be entered in the form ```{"Name":"LedOn", "Parameters":{}}``` )
- LedBlink : LED2 onboard Nucleo will blink for each message transmitted (message to be entered in the form ```{"Name":"LedBlink", "Parameters":{}}``` ).



### 3.4 Invoke direct methods from IoT Hub

The sample application supports [direct methods][lnk-direct-methods] to implement device management functionalities.

The following direct methods are supported by the sample application
- Quit : stop the application (```{"Method Name":"Quit", "Payload":{}}```)
- Reboot : reboot the board  (```{"Method Name":"Reboot", "Payload":{}}```)
- Firmware Update : trigger a Firmware update procedure by stopping current execution and downloading a new image (```{"Method Name":"FirmwareUpdate", "Payload":{"FwPackageUri":"https://..."}}```)

Direct methods can be executed using [DeviceExplorer][lnk-dev-exp] (Windows OS only) or [iothub-explorer][lnk-iot-exp], or directly from Azure portal. 


### 3.5 Modify desired properties in Device Twin

You can send configuration commands to [STM32L4 Discovery Kit][lnk-discovery] by editing [desired properties][lnk-desired-prop] in Device Twin.

The following desired property can be modified:
- DesiredTelemetryInterval : modifies the interval in seconds between the trasnmission of two consecutive device-to-cloud messages (default 2 seconds, max 30 seconds)



<a name="Nextsteps"></a>
# Next steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
-   [Save IoT Hub messages to Azure data storage](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
-   [Remote monitoring and notifications with ​​Logic ​​Apps](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)
-   [Device management with iothub-explorer](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-device-management-iothub-explorer)


[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[lnk-teraterm]:https://ttssh2.osdn.jp
[lnk-stlink]:http://www.st.com/content/st_com/en/products/embedded-software/development-tool-software/stsw-link004.html   
[lnk-minicom]:https://help.ubuntu.com/community/Minicom 
[lnk-iothub-explorer]:https://github.com/Azure/iothub-explorer
[lnk-direct-methods]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-direct-methods
[lnk-desired-prop]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-device-twins
[lnk-dev-man]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-device-management-overview
[lnk-fp-cld-azure]:http://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html
[lnk-dev-exp]:https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iot-exp]:https://github.com/Azure/iothub-explorer 
[lnk-discovery]:http://www.st.com/content/st_com/en/products/evaluation-tools/product-evaluation-tools/mcu-eval-tools/stm32-mcu-eval-tools/stm32-mcu-discovery-kits/b-l475e-iot01a.html



[1]: ./media/b-l475e-iot01a.png
[2]: ./media/b-l475e-iot01a-connect.png
[3]: ./media/b-l475e-iot01a-drag.png
[4]: ./media/b-l475e-iot01a-wifi.png
[5]: ./media/b-l475e-iot01a-conn-string.png


